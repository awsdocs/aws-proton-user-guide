--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# Update a service pipeline<a name="ag-svc-pipeline-update"></a>

There are four modes for updating a service pipeline as described in the following\. When using the AWS CLI, the `version-update-type` field defines the mode\. When using the AWS CLI, the `version-update-type` field defines the mode\. When using the console, these modes map to the **Edit pipeline** and **Update to recommended version**\.

  
`NO_DEPLOY`  
In this mode, there is no interaction with the underlying resources\. Only the requested metadata parameters are updated\.

  
`UPDATE_SPEC`  
In this mode, the service pipeline is updated to use the new specs provided\. Only requested parameters are updated\.

  
`MINOR_VERSION`  
In this mode, the service pipeline is updated to use the published, recommended \(latest\) minor version of the current major version in use\.

  
`MAJOR_VERSION`  
In this mode, the service pipeline is updated to use the published, recommended \(latest\) major and minor version of the current template\.

Update a service pipeline using the console as described in the following steps\.

1. From the [Proton console](https://console.aws.amazon.com/proton/), choose **Services**\.

1. From the list of services, click on the name of the service for which you want to update the pipeline\.

1. There are two tabs on the service detail page, **Overview** and **Pipeline**\. Click on **Pipeline**\.

1. If you want to update specs, click on **Edit Pipeline** and fill out each form and select **Next** until you complete the final form and click on **Update**\.

   If you want to update the template version and there's an **information icon** indicating a new version is available at **Pipeline template**, click on the name of the new template version\.

   1. Click on **Update to recommended version**\.

   1. Fill out each form and select **Next** until you complete the final form and click on **Update**\.

The following example shows how to use the AWS CLI to update a service pipeline\.

```
aws proton --region region-id update-service-pipeline --service-instance-name "instance-one" --service-name --spec file://service-spec.yaml --template-major-version-id "1" --template-minor-version-id "1" --version-update-type "MINOR_VERSION"
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
            "deploymentStatus": "IN_PROGRESS",
            "lastDeploymentAttemptTime": "2020-11-28T22:40:50.512000+00:00",
            "lastSuccessfulDeploymentTime": "2020-11-28T22:40:50.512000+00:00",
            "outputs": "{}",
            "provisionedStacks": ["AWSProton-simple-svc-cloudformation--HZYQRFWOSFPHCQZ"],
            "serviceTemplateArn": "arn:aws:proton:region-id:123456789012:service-template/simple-svc",
            "spec": "proton: ServiceSpec\npipeline:\n  my_sample_pipeline_optional_input: hello world\n  my_sample_pipeline_required_input: pipeline up\ninstances:\n- name: instance-one\n  environment: my-simple-env\n  spec:\n    my_sample_service_instance_optional_input: Ola\n    my_sample_service_instance_required_input: Ciao\n",
            "templateMajorVersionId": "1",
            "templateMinorVersionId": "0"
        },
        "serviceName": "simple-svc",
        "serviceTemplateArn": "arn:aws:proton:region-id:123456789012:service-template/simple-svc",
        "spec": "proton: ServiceSpec\npipeline:\n  my_sample_pipeline_optional_input: hello world\n  my_sample_pipeline_required_input: pipeline up\ninstances:\n- name: instance-one\n  environment: my-simple-env\n  spec:\n    my_sample_service_instance_required_input: hi\n    my_sample_service_instance_optional_input: ho\n",
        "status": "ACTIVE"
    }
}
```