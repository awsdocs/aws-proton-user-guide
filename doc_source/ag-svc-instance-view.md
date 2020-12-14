--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# View service instance data<a name="ag-svc-instance-view"></a>

You can view service instance detail data using either the console or the AWS CLI\.

You can view lists of service instances with details and view individual service instances with detail data by using the [Proton console](https://console.aws.amazon.com/proton/)\.

1. To view a list of the service instances, click on **Services instances** in the left\-hand menu\.

1. To view detail data, click on the name of a service instance\.

   View the detail data of your service instance\.

You can also use the AWS CLI for Proton by using the get or list operations as shown in the following examples\. You can get or list service instances\.

```
aws proton --region region-id list-service-instances
```

```
{
    "serviceInstances": [
        {
            "arn": "arn:aws:proton:region-id:123456789012:service/simple-svc/service-instance/instance-one",
            "createdAt": "2020-11-28T22:40:50.512000+00:00",
            "deploymentStatus": "SUCCEEDED",
            "environmentArn": "arn:aws:proton:region-id:123456789012:environment/simple-env",
            "lastDeploymentAttemptTime": "2020-11-28T22:40:50.512000+00:00",
            "lastSuccessfulDeploymentTime": "2020-11-28T22:40:50.512000+00:00",
            "serviceInstanceName": "instance-one",
            "serviceName": "simple-svc",
            "serviceTemplateArn": "arn:aws:proton:region-id:123456789012:service-template/fargate-service",
            "templateMajorVersionId": "1",
            "templateMinorVersionId": "0"
        }
    ]
}
```

```
aws proton --region region-id get-service-instance --service-instance-name "instance-one" --service-name "simple-svc"
```

```
{
    "serviceInstance": {
        "arn": "arn:aws:proton:region-id:123456789012:service/kstromsl-svc/service-instance/instance-one",
        "createdAt": "2020-11-28T22:40:50.512000+00:00",
        "deploymentStatus": "SUCCEEDED",
        "environmentArn": "arn:aws:proton:region-id:123456789012:environment/simple-env",
        "lastDeploymentAttemptTime": "2020-11-28T22:40:50.512000+00:00",
        "lastSuccessfulDeploymentTime": "2020-11-28T22:40:50.512000+00:00",
        "outputs": "{\"MyServiceInstanceParameter\":\"CFN-StoreServiceInstanceInputValue-bO7dNPxLiY2J\",\"MyServiceInstanceOptionalInputValue\":\"Ola\",\"MyServiceInstancesEnvironmentSampleOutputValue\":\"hello world\",\"MyServiceInstancesEnvironmentOtherSampleOutputValue\":\"hello hello\",\"MyServiceInstanceRequiredInputValue\":\"Ciao\"}",
        "provisionedStacks": ["AWSProton-simple-svc--cloudformation--HMACANJOICKTLNG"],
        "serviceInstanceName": "instance-one",
        "serviceName": "simple-svc",
        "serviceTemplateArn": "arn:aws:proton:region-id:123456789012:service-template/svc-simple",
        "spec": "proton: ServiceSpec\npipeline:\n  my_sample_pipeline_optional_input: hello world\n  my_sample_pipeline_required_input: pipeline up\ninstances:\n- name: instance-one\n  environment: my-simple-env\n  spec:\n    my_sample_service_instance_optional_input: Ola\n    my_sample_service_instance_required_input: Ciao\n",
        "templateMajorVersionId": "1",
        "templateMinorVersionId": "0"
    }
}
```