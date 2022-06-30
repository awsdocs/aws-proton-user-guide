# View service instance data<a name="ag-svc-instance-view"></a>

Learn to view AWS Proton service instance detail data\. You can use the console or the AWS CLI\.

A service instance belongs to a service\. You can only create or delete an instance within the context of service [edit](ag-svc-update.md), [create](ag-create-svc.md) and [delete](ag-svc-delete.md) actions\. To learn how to add and remove instances from a service, see [Edit a service](ag-svc-update.md)\.

------
#### [ AWS Management Console ]

**List and view service instance details using the [AWS Proton console](https://console.aws.amazon.com/proton/) as shown in the following steps\.**

1. To view a list of your service instances, choose **Services instances** in the navigation pane\.

1. To view detail data, choose the name of a service instance\.

   View your service instance detail data\.

------
#### [ AWS CLI ]

**List and view service instance details as shown in the following CLI example commands and responses\.**

Command:

```
$ aws proton list-service-instances
```

Response:

```
{
    "serviceInstances": [
        {
            "arn": "arn:aws:proton:region-id:123456789012:service/simple-svc/service-instance/instance-one",
            "createdAt": "2020-11-28T22:40:50.512000+00:00",
            "deploymentStatus": "SUCCEEDED",
            "environmentArn": "arn:aws:proton:region-id:123456789012:environment/simple-env",
            "lastDeploymentAttemptedAt": "2020-11-28T22:40:50.512000+00:00",
            "lastDeploymentSucceededAt": "2020-11-28T22:40:50.512000+00:00",
            "name": "instance-one",
            "serviceName": "simple-svc",
            "templateMajorVersion": "1",
            "templateMinorVersion": "0",
            "templateName": "fargate-service"
        }
    ]
}
```

Command:

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
        "createdAt": "2020-11-28T22:40:50.512000+00:00",
        "deploymentStatus": "SUCCEEDED",
        "environmentName": "simple-env",
        "lastDeploymentAttemptedAt": "2020-11-28T22:40:50.512000+00:00",
        "lastDeploymentSucceededAt": "2020-11-28T22:40:50.512000+00:00",
        "name": "instance-one",
        "serviceName": "simple-svc",
        "spec": "proton: ServiceSpec\npipeline:\n  my_sample_pipeline_optional_input: hello world\n  my_sample_pipeline_required_input: pipeline up\ninstances:\n- name: instance-one\n  environment: my-simple-env\n  spec:\n    my_sample_service_instance_optional_input: Ola\n    my_sample_service_instance_required_input: Ciao\n",
        "templateMajorVersion": "1",
        "templateMinorVersion": "0",
        "templateName": "svc-simple"
    }
}
```

------