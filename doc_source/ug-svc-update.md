# Edit a service<a name="ug-svc-update"></a>

You can make the following edits to a service\.
+ Edit the service description\.
+ Edit a service by adding and removing service instances\.

## Edit service description<a name="svc-update-metada"></a>

You can use the console or the AWS CLI to edit a service description\.

------
#### [ AWS Management Console ]

**Edit a service using the console as described in the following steps\.**

**In the list of services\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Services**\.

1. In the list of services, choose the radio button to the left of the service that you want to update\.

1. Choose **Edit**\.

1. In the **Configure service** page, fill out the form and choose **Next**\.

1. In the **Configure custom settings** page, choose **Next**\.

1. Review your edits and choose **Save changes**\.

**In the service detail page\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Services**\.

1. In the list of services, choose the name of the service that you want to edit\.

1. In the service detail page, choose **Edit**\.

1. In the **Configure service** page, fill out the form and choose **Next**\.

1. In the **Configure custom settings** page, choose **Next**\.

1. Review your edits and choose **Save changes**\.

------
#### [ AWS CLI ]

**Edit a description as shown in the following CLI example command and response\.**

Command:

```
aws proton update-service \
    --name "MySimpleService" 
    --description "Edit by updating description"
```

Response:

```
{
    "service": {
        "arn": "arn:aws:proton:region-id:123456789012:service/MySimpleService",
        "branchName": "main",
        "createdAt": "2021-03-12T22:39:42.318000+00:00",
        "description": "Edit by updating description",
        "lastModifiedAt": "2021-03-12T22:44:21.975000+00:00",
        "name": "MySimpleService",
        "repositoryConnectionArn": "arn:aws:codestar-connections:region-id:123456789012:connection/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "repositoryId": "my-repository/myorg-myapp",
        "status": "ACTIVE",
        "templateName": "fargate-service"
    }
}
```

------

## Edit by adding or removing service instances<a name="ug-svc-update-instances"></a>

For an AWS Proton service, you can add or delete service instances by submitting an edited [`spec`](#ug-svc-update-console-cli), if your request *isn't* made under the following conditions\. AWS Proton fails your request under these conditions\.
+ Your service and pipeline is already being edited or deleted when you submit the edit request\.
+ Your edited `spec` includes edits that modify the service pipeline or existing service instances that *aren't* to be deleted\.

Deletion\-failed instances are service instances in the `DELETE_FAILED` state\. When you request a service edit, AWS Proton attempts to remove the deletion\-failed instances for you, as part of the edit process\. If any of your service instances failed to delete, there might still be resources that are associated with the instances, even though they aren't visible from the console or AWS CLI\. Check your deletion\-failed instance infrastructure resources and clean them up so that AWS Proton can remove them for you\.

### Add or remove service instances<a name="ug-svc-add-delete"></a>

For the quota of service instances for a service, see [AWS Proton quotas](ag-limits.md)\. You also must maintain at least 1 service instance for your service after it's created\. During the update process, AWS Proton makes a count of the existing service instances and the instances to be added or removed\. Deletion\-failed instances are included in this count, and you need to account for them when you edit your `spec`\.

After you submit a service edit to delete and add service instances, AWS Proton takes the following actions\.
+ Sets the service to `UPDATE_IN_PROGRESS`\.
+ If the service has a pipeline, sets its status to `IN_PROGRESS` and blocks pipeline actions\.
+ Sets any service instances that are to be deleted to `DELETE_IN_PROGRESS`\.
+ Blocks service actions\.
+ Blocks actions on service instances that are marked for deletion\.
+ Creates new service instances\.
+ Deletes instances that you listed for deletion\.
+ Attempts to remove deletion\-failed instances\.
+ After additions and deletions are complete, re\-provisions the service pipeline \(if there is one\), sets your service to `ACTIVE` and enables service and pipeline actions\.

AWS Proton attempts to re\-mediate failure modes as follows\.
+ If one or more service instances *failed to be created*, AWS Proton tries to de\-provision all of the newly created service instances and reverts the `spec` to the previous state\. It *doesn't* delete any service instances, and it *doesn't* modify the pipeline in any way\.
+ If one or more service instances *failed to be deleted*, AWS Proton re\-provisions the pipeline without the deleted instances\. The `spec` is updated to include the added instances and to exclude the instances that were marked for deletion\.
+ If the *pipeline fails provisioning*, a rollback *isn't* attempted and both the service and pipeline reflect a failed update state\.

#### Tagging and service edits<a name="ug-svc-update-tagging"></a>

When you add service instances as part of your service edit, AWS managed tags propagate to and are automatically created for the new instances and provisioned resources\. If you create new tags, those tags are only applied to the new instances\. Existing service customer managed tags also propagate to the new instances\. For more information, see [AWS Proton resources and tagging](resources.md)\.

#### Use the console or AWS CLI to edit a service<a name="ug-svc-update-console-cli"></a>

You can use the AWS Proton console and AWS CLI to edit a service by adding and removing instances\.

------
#### [ AWS Management Console ]

**Edit your service by adding or removing an instance using the console\.**

In the [AWS Proton console](https://console.aws.amazon.com/proton/)

1. In the navigation pane, choose **Services**\.

1. choose the service that you want to edit\.

1. Choose **Edit**\.

1. \(Optional\) On the **Configure service** page, edit the service name or description and, at the lower right corner of the page, choose **Next**\.

1. On the **Configure custom settings** page, choose **Delete** to delete a service instance and choose **Add new instance** to add a service instance and fill out the form\.

1. On the lower right\-hand corner of the page, choose **Next**\.

1. Review your update and choose **Save changes**\.

1. A modal asks you to verify deletion of service instances\. Follow the instructions and choose **Yes, delete**\.

1. In the service detail page, view the status details for your service\.

------
#### [ AWS CLI ]

**Add and delete service instances with an edited `spec` as shown in the following AWS CLI example commands and responses\.**

When you use the CLI, your `spec` must *exclude* the service instances to delete and *include* both the service instances to add and the existing service instances that you *haven't* marked for deletion\.

The following listing shows the example `spec` before the edit and a list of the service instances deployed by the spec\. This spec was used in the previous example for editing a service description\.

Spec:

```
proton: ServiceSpec

pipeline:
  my_sample_pipeline_optional_input: "abc"
  my_sample_pipeline_required_input: "123"

instances:
  - name: "my-instance"
    environment: "simple-env"
    spec:
      my_sample_service_instance_optional_input: "def"
      my_sample_service_instance_required_input: "456"
  - name: "my-other-instance"
    environment: "simple-env"
    spec:
      my_sample_service_instance_required_input: "789"
```

The following example `list-service-instances` command and response shows the active instances prior to adding or deleting a service instance\.

Command:

```
aws proton list-service-instances \
    --service-name "MySimpleService"
```

Response:

```
{
    "serviceInstances": [
        {
            "arn": "arn:aws:proton:region-id:123456789012:service/MySimpleService/service-instance/my-other-instance",
            "createdAt": "2021-03-12T22:39:42.318000+00:00",
            "deploymentStatus": "SUCCEEDED",
            "environmentName": "simple-env",
            "lastDeploymentAttemptedAt": "2021-03-12T22:39:43.109000+00:00",
            "lastDeploymentSucceededAt": "2021-03-12T22:39:43.109000+00:00",
            "name": "my-other-instance",
            "serviceName": "example-svc",
            "templateMajorVersion": "1",
            "templateMinorVersion": "0",
            "templateName": "fargate-service"
        },
        {
            "arn": "arn:aws:proton:region-id:123456789012:service/MySimpleService/service-instance/my-instance",
            "createdAt": "2021-03-12T22:39:42.318000+00:00",
            "deploymentStatus": "SUCCEEDED",
            "environmentName": "simple-env",
            "lastDeploymentAttemptedAt": "2021-03-12T22:39:43.160000+00:00",
            "lastDeploymentSucceededAt": "2021-03-12T22:39:43.160000+00:00",
            "name": "my-instance",
            "serviceName": "example-svc",
            "serviceTemplateArn": "arn:aws:proton:region-id:123456789012:service-template/fargate-service",
            "templateMajorVersion": "1",
            "templateMinorVersion": "0",
            "templateName": "fargate-service"
        }
    ]
}
```

The following listing shows the example edited `spec` that's used to delete and add an instance\. The existing instance named `my-instance` is removed and a new instance named `yet-another-instance` is added\.

Spec:

```
proton: ServiceSpec

pipeline:
  my_sample_pipeline_optional_input: "abc"
  my_sample_pipeline_required_input: "123"

instances:
  - name: "my-other-instance"
    environment: "simple-env"
    spec:
      my_sample_service_instance_required_input: "789"
  - name: "yet-another-instance"
    environment: "simple-env"
    spec:
      my_sample_service_instance_required_input: "789"
```

You can use `"${Proton::CURRENT_VAL}"` to indicate which parameter values to preserve from the original `spec`, if the values exist in the `spec`\. Use `get-service` to view the original `spec` for a service, as described in [View service data](ug-svc-view.md)\.

The following listing shows how you can use `"${Proton::CURRENT_VAL}"` to ensure your `spec` *doesn't* include parameter values changes for the existing services instances to remain\.

Spec:

```
proton: ServiceSpec

pipeline:
  my_sample_pipeline_optional_input: "${Proton::CURRENT_VAL}"
  my_sample_pipeline_required_input: "${Proton::CURRENT_VAL}"

instances:
  - name: "my-other-instance"
    environment: "simple-env"
    spec:
      my_sample_service_instance_required_input: "${Proton::CURRENT_VAL}"
  - name: "yet-another-instance"
    environment: "simple-env"
    spec:
      my_sample_service_instance_required_input: "789"
```

The next listing shows the CLI command and response to edit the service\.

Command:

```
aws proton update-service \
    --name "MySimpleService" \
    --description "Edit by adding and deleting a service instance" \
    --spec "file://spec.yaml"
```

Response:

```
{
    "service": {
        "arn": "arn:aws:proton:region-id:123456789012:service/MySimpleService",
        "branchName": "main",
        "createdAt": "2021-03-12T22:39:42.318000+00:00",
        "description": "Edit by adding and deleting a service instance",
        "lastModifiedAt": "2021-03-12T22:55:48.169000+00:00",
        "name": "MySimpleService",
        "repositoryConnectionArn": "arn:aws:codestar-connections:region-id:123456789012:connection/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "repositoryId": "my-repository/myorg-myapp",
        "status": "UPDATE_IN_PROGRESS",
        "templateName": "fargate-service"
    }
}
```

The following `list-service-instances` command and response confirms that the existing instance named `my-instance` is removed and a new instance named `yet-another-instance` is added\.

Command:

```
aws proton list-service-instances \
    --service-name "MySimpleService"
```

Response:

```
{
    "serviceInstances": [
        {
            "arn": "arn:aws:proton:region-id:123456789012:service/MySimpleService/service-instance/yet-another-instance",
            "createdAt": "2021-03-12T22:39:42.318000+00:00",
            "deploymentStatus": "SUCCEEDED",
            "environmentName": "simple-env",
            "lastDeploymentAttemptedAt": "2021-03-12T22:56:01.565000+00:00",
            "lastDeploymentSucceededAt": "2021-03-12T22:56:01.565000+00:00",
            "name": "yet-another-instance",
            "serviceName": "MySimpleService",
            "templateMajorVersion": "1",
            "templateMinorVersion": "0",
            "templateName": "fargate-service"
        },
        {
            "arn": "arn:aws:proton:region-id:123456789012:service/MySimpleService/service-instance/my-other-instance",
            "createdAt": "2021-03-12T22:39:42.318000+00:00",
            "deploymentStatus": "SUCCEEDED",
            "environmentName": "simple-env",
            "lastDeploymentAttemptedAt": "2021-03-12T22:39:43.109000+00:00",
            "lastDeploymentSucceededAt": "2021-03-12T22:39:43.109000+00:00",
            "name": "my-other-instance",
            "serviceName": "MySimpleService",
            "templateMajorVersion": "1",
            "templateMinorVersion": "0",
            "templateName": "fargate-service"
        }
    ]
}
```

------