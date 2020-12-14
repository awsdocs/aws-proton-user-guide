--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# Delete<a name="ug-svc-delete"></a>

You can delete a service by using the console or the AWS CLI\.

Delete a service using the console as described in the following steps\.

**From the service detail page\.**

1. From the [Proton console](https://console.aws.amazon.com/proton/), choose **Services**\.

1. From the list of services, click on the name of the service you want to delete\.

1. Click on **Actions** and then **Delete**\.

You can also use the AWS CLI for Proton to delete a service as shown in the following steps\.

```
aws proton --region region-id delete-service --service-name "simple-svc"
```

```
{
    "service": {
        "arn": "arn:aws:proton:region-id:123456789012:service/simple-svc",
        "createdAt": "2020-11-28T22:40:50.512000+00:00",
        "lastModifiedAt": "2020-11-29T00:30:39.248000+00:00",
        "serviceName": "simple-svc",
        "serviceTemplateArn": "arn:aws:proton:region-id:123456789012:service-template/fargate-service",
        "status": "DELETE_STARTED"
    }
}
```