# Delete<a name="ug-svc-delete"></a>

You can delete a service by using the console or the AWS CLI\.

Delete a service using the console as described in the following steps\.

**In the service detail page\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Services**\.

1. In the list of services, choose the name of the service that you want to delete\.

1. Choose **Actions** and then **Delete**\.

1. A modal prompts you to confirm the delete action\.

1. Follow the instructions and choose **Yes, delete**\.

You can also use the AWS CLI for AWS Proton to delete a service as shown in the following command and response\.

Command:

```
aws proton delete-service --name "simple-svc"
```

Response:

```
{
    "service": {
        "arn": "arn:aws:proton:region-id:123456789012:service/simple-svc",
        "createdAt": "2020-11-28T22:40:50.512000+00:00",
        "lastModifiedAt": "2020-11-29T00:30:39.248000+00:00",
        "name": "simple-svc",
        "status": "DELETE_IN_PROGRESS",
        "templateName": "fargate-service"
    }
}
```