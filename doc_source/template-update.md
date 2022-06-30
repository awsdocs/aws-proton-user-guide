# Update a template<a name="template-update"></a>

You can update a template as described in the following list\.
+ Edit the `description` or `display name` of a template when you use either the console or AWS CLI\. You *can't* edit the `name` of a template\.
+ Update the status of a template minor version when you use either the console or AWS CLI\. You can only change the status from `DRAFT` to `PUBLISHED`\.
+ Edit the display name and description of a minor or major version of a template when you use the AWS CLI\.

------
#### [ AWS Management Console ]

Edit a template description and display name using the console as described in the following steps\.

**In the list of templates\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **\(Environment or Service\) Templates**\.

1. In the list of templates, choose the radio button to the left of the template that you want to update the description or display name for\.

1. Choose **Actions** and then **Edit**\.

1. In the **Edit \(environment or service\) template** page, in the **Template details** section, enter your edits in the form and choose **Save changes**\.

Change the status of a minor version of a template using the console to publish a template as described in the following\. You can only change the status from `DRAFT` to `PUBLISHED`\.

**In the \(environment or service\) template detail page\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **\(Environment or Service\) templates**\.

1. In the list of templates, choose the name of the template that you want to update the status of a minor version from **Draft** to **Published**\.

1. In the \(environment or service\) template detail page, in the **Template versions** section, select the radio button to the left of the minor version that you want to publish\.

1. Choose **Publish** in the **Template versions** section\. The status changes from **Draft** to **Published**\.

------
#### [ AWS CLI ]

The following example command and response shows how you can edit the description of an environment template\.

Run the following command\.

```
$ aws proton update-environment-template \
    --name "simple-env" \
    --description "A single VPC with public access"
```

Response:

```
{
    "environmentTemplate": {
        "arn": "arn:aws:proton:region-id:123456789012:environment-template/simple-env",
        "createdAt": "2020-11-28T22:02:10.651000+00:00",
        "description": "A single VPC with public access",
        "displayName": "simple-env",
        "lastModifiedAt": "2020-11-29T16:11:18.956000+00:00",
        "majorVersion": "1",
        "minorVersion": "0",
        "recommendedMinorVersion": "0",
        "schema": "schema:\n  format:\n    openapi: \"3.0.0\"\n  environment_input_type: \"MyEnvironmentInputType\"\n  types:\n    MyEnvironmentInputType:\n      type: object\n      description: \"Input properties for my environment\"\n      properties:\n        my_sample_input:\n          type: string\n          description: \"This is a sample input\"\n          default: \"hello world\"\n        my_other_sample_input:\n          type: string\n          description: \"Another sample input\"\n      required:\n        - my_other_sample_input\n",
        "status": "PUBLISHED",
        "statusMessage": "",
        "templateName": "simple-env"
    }
}
```

You can also use the AWS CLI to update service templates\. See [Register and publish service templates](template-create.md#svc-template-v1), step 5, for an example of updating the status of a minor version of a service template\.

------