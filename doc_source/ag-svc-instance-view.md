# View service instance data<a name="ag-svc-instance-view"></a>

You can view service instance detail data using either the console or the AWS CLI\.

You can view lists of service instances with details and view individual service instances with detail data by using the [AWS Proton console](https://console.aws.amazon.com/proton/)\.

1. To view a list of the service instances, choose **Services instances** in the navigation pane\.

1. To view detail data, choose the name of a service instance\.

   View the detail data of your service instance\.

You can also use the AWS CLI for AWS Proton by using the get or list operations as shown in the following example commands and responses\. You can *get* or *list* service instances\.

Command:

```
aws proton list-service-instances
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
aws proton get-service-instance --name "instance-one" --service-name "simple-svc"
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
        "templateName": "svc-simple",
        "spec": "proton: ServiceSpec\npipeline:\n  my_sample_pipeline_optional_input: hello world\n  my_sample_pipeline_required_input: pipeline up\ninstances:\n- name: instance-one\n  environment: my-simple-env\n  spec:\n    my_sample_service_instance_optional_input: Ola\n    my_sample_service_instance_required_input: Ciao\n",
        "templateMajorVersion": "1",
        "templateMinorVersion": "0"
    }
}
```