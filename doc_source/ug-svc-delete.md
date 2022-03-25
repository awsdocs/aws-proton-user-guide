# Delete<a name="ug-svc-delete"></a>

You can delete a service, with its instances and pipeline, by using the console or the AWS CLI\.

------
#### [ AWS Management Console ]

**Delete a service using the console as described in the following steps\.**

**In the service detail page\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Services**\.

1. In the list of services, choose the name of the service that you want to delete\.

1. Choose **Actions** and then **Delete**\.

1. A modal prompts you to confirm the delete action\.

1. Follow the instructions and choose **Yes, delete**\.

------
#### [ AWS CLI ]

**Delete a service as shown in the following CLI example command and response\.**

Command:

```
$ aws proton delete-service \
    --name "simple-svc"
```

Response:

```
{
    "service": {
        "arn": "arn:aws:proton:region-id:123456789012:service/simple-svc",
        "branchName": "mainline",
        "createdAt": "2020-11-28T22:40:50.512000+00:00",
        "description": "Edit by updating description",
        "lastModifiedAt": "2020-11-29T00:30:39.248000+00:00",
        "name": "simple-svc",
        "repositoryConnectionArn": "arn:aws:codestar-connections:region-id:123456789012:connection/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "repositoryId": "myorg/myapp",
        "status": "DELETE_IN_PROGRESS",
        "templateName": "fargate-service"
    }
}
```

------