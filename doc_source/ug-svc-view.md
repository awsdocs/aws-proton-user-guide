--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# View service data<a name="ug-svc-view"></a>

You can view service detail data using either the console or the AWS CLI\.

You can view lists of services with details and view individual services with detail data by using the [Proton console](https://console.aws.amazon.com/proton/)\.

1. To view a list of the services, click on **Services**\.

1. To view detail data, click on the name of your service\.

   View your service detail\.

You can also use the AWS CLI for Proton by using the get or list operations as shown in the following example\. You can get or list services\.

```
aws proton --region region-id get-service --service-name "simple-svc"
```

```
{
    "service": {
        "arn": "arn:aws:proton:region-id:123456789012:service/simple-svc",
        "createdAt": "2020-11-28T22:40:50.512000+00:00",
        "lastModifiedAt": "2020-11-28T22:44:51.207000+00:00",
        "pipeline": {
            "arn": "arn:aws:proton:region-id:123456789012:service/simple-svc/pipeline",
            "createdAt": "2020-11-28T22:40:50.512000+00:00",
            "deploymentStatus": "SUCCEEDED",
            "lastDeploymentAttemptTime": "2020-11-28T22:40:50.512000+00:00",
            "lastSuccessfulDeploymentTime": "2020-11-28T22:40:50.512000+00:00",
            "outputs": "{\"MySamplePipelineInputValue\":\"hello\",\"MyPipelineParameter\":\"CFN-StorePipelineInputValues-QnVmtxACj022\",\"MyOtherSamplePipelineInputValue\":\"bye\"}",
            "provisionedStacks": [
                "AWSProton-simple-svc-cloudformation--HZYQRFWOSFPHCQZ"
            ],
            "serviceTemplateArn": "arn:aws:proton:region-id:123456789012:service-template/svc-simple",
            "spec": "proton: ServiceSpec\npipeline:\n  my_sample_pipeline_required_input: hello\n  my_sample_pipeline_optional_input: bye\ninstances:\n- name: instance-kstromsl\n  environment: my-simple-env\n  spec:\n    my_sample_service_instance_required_input: hi\n    my_sample_service_instance_optional_input: ho\n",
            "templateMajorVersionId": "1",
            "templateMinorVersionId": "1"
        },
        "serviceName": "simple-svc",
        "serviceTemplateArn": "arn:aws:proton:region-id:123456789012:service-template/svc-simple",
        "spec": "proton: ServiceSpec\npipeline:\n  my_sample_pipeline_required_input: hello\n  my_sample_pipeline_optional_input: bye\ninstances:\n- name: instance-kstromsl\n  environment: my-simple-env\n  spec:\n    my_sample_service_instance_required_input: hi\n    my_sample_service_instance_optional_input: ho\n",
        "status": "ACTIVE"
    }
}
```