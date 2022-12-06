# Update a service instance<a name="ag-svc-instance-update"></a>

Learn to update an AWS Proton service instance and cancel the update\.

A service instance belongs to a service\. You can only create or delete an instance within the context of service [edit](ag-svc-update.md), [create](ag-create-svc.md) and [delete](ag-svc-delete.md) actions\. To learn how to add and remove instances from a service, see [Edit a service](ag-svc-update.md)\.

There are four modes for updating a service instance as described in the following list\. When using the AWS CLI, the `deployment-type` field defines the mode\. When using the console, these modes map to the **Edit** and the **Update to latest minor version** and **Update to latest major version** actions that drop down from **Actions** in the service instance detail page\.


`NONE`
In this mode, a deployment *doesn't* occur\. Only the requested metadata parameters are updated\.


`CURRENT_VERSION`
In this mode, the service instance is deployed and updated with the new spec that you provide\. Only requested parameters are updated\. *Don’t* include minor or major version parameters when you use this `deployment-type`\.


`MINOR_VERSION`
In this mode, the service instance is deployed and updated with the published, recommended \(latest\) minor version of the current major version in use by default\. You can also specify a different minor version of the current major version in use\.


`MAJOR_VERSION`
In this mode, the service instance is deployed and updated with the published, recommended \(latest\) major and minor version of the current template by default\. You can also specify a different major version that is higher than the major version in use and a minor version \(optional\)\.

You can attempt to cancel a service instance update deployment if the `deploymentStatus` is `IN_PROGRESS`\. AWS Proton attempts to cancel the deployment\. Successful cancellation *isn’t* guaranteed\.

When you cancel an update deployment, AWS Proton attempts to cancel the deployment as listed in the following steps\.
+ Sets the deployment state to `CANCELLING`\.
+ Stops the deployment in process and deletes any new resources that were created by the deployment when `IN_PROGRESS`\.
+ Sets the deployment state to `CANCELLED`\.
+ Reverts the state of the resource to what it was before the deployment was started\.

For more information on cancelling a service instance deployment, see [CancelServiceInstanceDeployment](https://docs.aws.amazon.com/proton/latest/APIReference/API_CancelServiceInstanceDeployment.html) in the *AWS Proton API Reference*\.

**Use the console or AWS CLI to make updates or cancel update deployments\.**

------
#### [ AWS Management Console ]

**Update a service instance using the console by following these steps\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Service instances** in the navigation pane\.

1. In the list of service instances, choose the name of the service instance that you want to update\.

1. Choose **Actions** and then choose one of the update options, **Edit** to update spec or **Actions** and then **Update to latest minor version**, or **Update to latest major version**\.

1. Fill out each form and choose **Next** until you reach the **Review** page\.

1. Review your edits and choose **Update**\.

------
#### [ AWS CLI ]

**Update a service instance to a new minor version as shown in the CLI example commands and responses\.**

When you update your service instance with a modified `spec`, you can use `"${Proton::CURRENT_VAL}"` to indicate which parameter values to preserve from the original `spec`, if the values exist in the `spec`\. Use `get-service` to view the original `spec` for a service instance, as described in [View service data](ag-svc-view.md)\.

The following example shows how you can use `"${Proton::CURRENT_VAL}"` in a `spec`\.

Spec:

```
proton: ServiceSpec

pipeline:
  my_sample_pipeline_optional_input: "${Proton::CURRENT_VAL}"
  my_sample_pipeline_required_input: "${Proton::CURRENT_VAL}"

instances:
  - name: "my-instance"
    environment: "simple-env"
    spec:
      my_sample_service_instance_optional_input: "${Proton::CURRENT_VAL}"
      my_sample_service_instance_required_input: "${Proton::CURRENT_VAL}"
  - name: "my-other-instance"
    environment: "simple-env"
    spec:
      my_sample_service_instance_required_input: "789"
```

Command: to update

```
$ aws proton update-service-instance \
    --name "instance-one" \
    --service-name "simple-svc" \
    --spec "file://service-spec.yaml" \
    --template-major-version "1" \
    --template-minor-version "1" \
    --deployment-type "MINOR_VERSION"
```

Response:

```
{
    "serviceInstance": {
        "arn": "arn:aws:proton:region-id:123456789012:service/simple-svc/service-instance/instance-one",
        "createdAt": "2021-04-02T21:29:59.962000+00:00",
        "deploymentStatus": "IN_PROGRESS",
        "environmentName": "arn:aws:proton:region-id:123456789012:environment/simple-env",
        "lastDeploymentAttemptedAt": "2021-04-02T21:38:00.823000+00:00",
        "lastDeploymentSucceededAt": "2021-04-02T21:29:59.962000+00:00",
        "name": "instance-one",
        "serviceName": "simple-svc",
        "templateMajorVersion": "1",
        "templateMinorVersion": "0",
        "templateName": "svc-simple"
    }
}
```

Command: to get and confirm status

```
$ aws proton get-service-instance \
    --name "instance-one" \
    --service-name "simple-svc"
```

Response:

```
{
    "serviceInstance": {
        "arn": "arn:aws:proton:region-id:123456789012:service/simple-svc/service-instance/instance-one",
        "createdAt": "2021-04-02T21:29:59.962000+00:00",
        "deploymentStatus": "SUCCEEDED",
        "environmentName": "simple-env",
        "lastDeploymentAttemptedAt": "2021-04-02T21:38:00.823000+00:00",
        "lastDeploymentSucceededAt": "2021-04-02T21:38:00.823000+00:00",
        "name": "instance-one",
        "serviceName": "simple-svc",
        "spec": "proton: ServiceSpec\n\npipeline:\n  my_sample_pipeline_optional_input: \"abc\"\n  my_sample_pipeline_required_input: \"123\"\n\ninstances:\n  - name: \"instance-one\"\n    environment: \"simple-env\"\n    spec:\n      my_sample_service_instance_optional_input: \"def\"\n      my_sample_service_instance_required_input: \"456\"\n  - name: \"my-other-instance\"\n    environment: \"kls-simple-env\"\n    spec:\n      my_sample_service_instance_required_input: \"789\"\n",
        "templateMajorVersion": "1",
        "templateMinorVersion": "1",
        "templateName": "svc-simple"
    }
}
```

------

------
#### [ AWS Management Console ]

**Cancel a service instance deployment using the console as shown in the following steps\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Service instances** in the navigation pane\.

1. In the list of service instances, choose the name of the service instance with the deployment update that you want to cancel\.

1. If your update deployment status is **In progress**, in the service instance detail page, choose **Actions** and then **Cancel deployment**\.

1. A modal asks you to confirm the cancellation\. Choose **Cancel deployment**\.

1. Your update deployment status is set to **Cancelling** and then **Cancelled** to complete the cancellation\.

------
#### [ AWS CLI ]

**Cancel an IN\_PROGRESS service instance deployment update to new minor version 2 as shown in the following CLI example commands and responses\.**

A wait condition is included in the template used for this example so that the cancellation starts before the update deployment succeeds\.

Command: to cancel

```
$ aws proton cancel-service-instance-deployment \
    --service-instance-name "instance-one" \
    --service-name "simple-svc"
```

Response:

```
{
    "serviceInstance": {
        "arn": "arn:aws:proton:region-id:123456789012:service/simple-svc/service-instance/instance-one",
        "createdAt": "2021-04-02T21:29:59.962000+00:00",
        "deploymentStatus": "CANCELLING",
        "environmentName": "simple-env",
        "lastDeploymentAttemptedAt": "2021-04-02T21:45:15.406000+00:00",
        "lastDeploymentSucceededAt": "2021-04-02T21:38:00.823000+00:00",
        "name": "instance-one",
        "serviceName": "simple-svc",
        "spec": "proton: ServiceSpec\npipeline:\n  my_sample_pipeline_optional_input: abc\n  my_sample_pipeline_required_input: '123'\ninstances:\n- name: my-instance\n  environment: MySimpleEnv\n  spec:\n    my_sample_service_instance_optional_input: def\n    my_sample_service_instance_required_input: '456'\n- name: my-other-instance\n  environment: MySimpleEnv\n  spec:\n    my_sample_service_instance_required_input: '789'\n",
        "templateMajorVersion": "1",
        "templateMinorVersion": "1",
        "templateName": "svc-simple"
    }
}
```

Command: to get and confirm status

```
$ aws proton get-service-instance \
    --name "instance-one" \
    --service-name "simple-svc"
```

Response:

```
{
    "serviceInstance": {
        "arn": "arn:aws:proton:region-id:123456789012:service/simple-svc/service-instance/instance-one",
        "createdAt": "2021-04-02T21:29:59.962000+00:00",
        "deploymentStatus": "CANCELLED",
        "deploymentStatusMessage": "User initiated cancellation.",
        "environmentName": "simple-env",
        "lastDeploymentAttemptedAt": "2021-04-02T21:45:15.406000+00:00",
        "lastDeploymentSucceededAt": "2021-04-02T21:38:00.823000+00:00",
        "name": "instance-one",
        "serviceName": "simple-svc",
        "spec": "proton: ServiceSpec\n\npipeline:\n  my_sample_pipeline_optional_input: \"abc\"\n  my_sample_pipeline_required_input: \"123\"\n\ninstances:\n  - name: \"instance-one\"\n    environment: \"simple-env\"\n    spec:\n      my_sample_service_instance_optional_input: \"def\"\n      my_sample_service_instance_required_input: \"456\"\n  - name: \"my-other-instance\"\n    environment: \"kls-simple-env\"\n    spec:\n      my_sample_service_instance_required_input: \"789\"\n",
        "templateMajorVersion": "1",
        "templateMinorVersion": "1",
        "templateName": "svc-simple"
    }
}
```

------