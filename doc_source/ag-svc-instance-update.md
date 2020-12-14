--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# Update a service instance<a name="ag-svc-instance-update"></a>

There are four modes for updating a service instance as described in the following\. When using the AWS CLI, the `version-update-type` field defines the mode\. When using the console, these modes map to the **Update spec**, **Update to latest minor version** and **Update to latest major version** actions that drop down from **Actions** in the service instance detail page\.

  
`NO_DEPLOY`  
In this mode, there is no interaction with the underlying resources\. Only the requested metadata parameters are updated\.

  
`UPDATE_SPEC`  
In this mode, the service instance is updated to use the new specs provided\. Only requested parameters are updated\.

  
`MINOR_VERSION`  
In this mode, the service instance is updated to use the published, recommended \(latest\) minor version of the current major version in use\.

  
`MAJOR_VERSION`  
In this mode, the service instance is updated to use the published, recommended \(latest\) major and minor version of the current template\.

Update a service instance using the console as described in the following steps\.

1. From the [Proton console](https://console.aws.amazon.com/proton/), choose **Service instances** in the left\-hand menu\.

1. From the list of service instances, click on the name of the service instance that you want to update\.

1. Click on **Actions** and then choose one of the update options, **Update spec**, **Update to latest minor version** or **Update to latest major version**\.

1. Fill out each form and select **Next** until you complete the final form and click on **Update**\.

The following example shows how to use the AWS CLI to update a service instance\.

```
aws proton --region region-id update-service-instance --service-instance-name "instance-one" --service-name "simple-svc" --spec file://service-spec.yaml --template-major-version-id "1" --template-minor-version-id "0" --version-update-type "UPDATE_SPEC"
```

```
{
    "serviceInstance": {
        "arn": "arn:aws:proton:region-id:123456789012:service/simple-svc/service-instance/instance-one",
        "createdAt": "2020-11-28T22:40:50.512000+00:00",
        "deploymentStatus": "IN_PROGRESS",
        "environmentArn": "arn:aws:proton:region-id:123456789012:environment/simple-env",
        "lastDeploymentAttemptTime": "2020-11-28T22:40:50.512000+00:00",
        "lastSuccessfulDeploymentTime": "2020-11-28T22:40:50.512000+00:00",
        "serviceInstanceName": "instance-one",
        "serviceName": "svc-simple",
        "serviceTemplateArn": "arn:aws:proton:region-id:123456789012:service-template/svc-simple",
        "templateMajorVersionId": "1",
        "templateMinorVersionId": "0"
    }
}
```