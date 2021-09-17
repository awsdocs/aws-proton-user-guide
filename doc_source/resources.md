# AWS Proton resources and tagging<a name="resources"></a>

AWS Proton resources that are assigned an Amazon Resource Name \(ARN\) include environment templates and their major and minor versions, service templates and their major and minor versions, environments, services and service instances\. You can tag these resources to help you organize and identify them\. You can use tags to categorize resources by purpose, owner, environment, or other criteria\. For more information, see [ Tagging Strategies](https://aws.amazon.com/answers/account-management/aws-tagging-strategies/)\. To track and manage your AWS Proton resources, you can use the tagging features of as described in the following sections\. 

## AWS tagging<a name="aws-tags"></a>

You can assign metadata to your AWS resources in the form of tags\. Each tag consists of a customer defined key and optional value\. Tags can help you manage, identify, organize, search for, and filter resources\.

**Important**  
Do not add personally identifiable information \(PII\) or other confidential or sensitive information in tags\. Tags are accessible to many AWS services, including billing\. Tags are not intended to be used for private or sensitive data\. 

**Each tag has two parts\.**
+ A tag key \(for example, `CostCenter`, `Environment`, or `Project`\)\. Tag keys are case sensitive\.
+ A tag value \(optional\) \(for example, `111122223333` or `Production`\)\. Like tag keys, tag values are case sensitive\.

**The following basic naming and usage requirements apply to tags\.**
+ Each resource can have a maximum of 50 user created tags\.
**Note**  
System created tags that begin with the `aws:` prefix are reserved for AWS use, and do not count against this limit\. You can't edit or delete a tag that begins with the `aws:` prefix\.
+ For each resource, each tag key must be unique, and each tag key can have only one value\.
+ The tag key must be a minimum of 1 and a maximum of 128 Unicode characters in UTF\-8\.
+ The tag value must be a minimum of 1 and a maximum of 256 Unicode characters in UTF\-8\.
+ Allowed characters in tags are letters, numbers, spaces representable in UTF\-8, and the following characters:\* \_ \. : / = \+ \- @\.

## AWS Proton tagging<a name="proton-tags"></a>

With AWS Proton, you can use both the tags that you create as well as the tags that AWS Proton automatically generates for you\.

### AWS Proton AWS managed tags<a name="auto-tags"></a>

When you create an AWS Proton resource, AWS Proton automatically generates AWS managed tags for your new resource as shown in the following diagram\. AWS managed tags propagate to the AWS Proton resources that your new resource creates or deploys\.

![\[A diagram that describes the AWS managed tag propagation.\]](http://docs.aws.amazon.com/proton/latest/userguide/images/tag-diag.png)

If provisioned resources, such as those defined in service and environment templates, support AWS tagging, the AWS managed tags propagate as customer managed tags to provisioned resources\. These tags won't propagate to a provisioned resource that doesn't support AWS tagging\.

AWS Proton applies tags to your resources by AWS Proton accounts, registered templates and deployed environments, as well as services and service instances as described in the following table\. You can use AWS managed tags to view and manage your AWS Proton resources, but you can't modify them\.


| AWS managed tag key | Propagated customer managed key | Description | 
| --- | --- | --- | 
|  `aws:proton:account`  |  `proton:account`  |  The ARN of the account that creates and deploys AWS Proton resources\.  | 
|  `aws:proton:template`  |  `proton:template`  |  The ARN of a selected template\.  | 
|  `aws:proton:service`  |  `proton:service`  |  The ARN of a selected service\.  | 
|  `aws:proton:service-instance`  |  `proton:service-instance`  |  The ARN of a selected service instance\.  | 
|  `aws:proton:environment`  |  `proton:environment`  |  The ARN of a selected environment\.  | 

The following is an example of an AWS managed tag for an AWS Proton resource\.

```
"aws:proton:template" = "arn:aws:proton:region-id:account-id:environment-template/env-template"
```

The following is an example of a customer managed tag applied to a provisioned resource that was propagated from an AWS managed tag\.

```
"proton:environment:database" = "arn:aws:proton:region-id:account-id:rds/env-db"
```

### Customer managed tags<a name="user-tags"></a>

Each AWS Proton resource has a maximum quota of 50 customer managed tags\. Customer managed tags propagate to child AWS Proton resources in the same way that AWS managed tags do, except they don't propagate to existing AWS Proton resources or to provisioned resources\. If you apply a new tag to an AWS Proton resource with existing child resources and you want the existing child resources to be tagged with the new tag, you need to tag each existing child resource manually, using the console or AWS CLI\.

### Create tags using the console<a name="console-tags"></a>

When you create an AWS Proton resource using the console, you're given the opportunity to create customer managed tags either on the first or second page of the create procedure as shown in the following console snapshot\. choose **Add new tag**, enter the key and value and proceed\.

![\[A snapshot of the console create tag interface.\]](http://docs.aws.amazon.com/proton/latest/userguide/images/tag-create.PNG)

After you create a new resource using the AWS Proton console, you can view its list of AWS managed and customer managed tags from the detail page\.

**Create or edit a tag**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), open an AWS Proton resource detail page where you can see a list of tags\.

1. Choose **Manage tags**\.

1. In the **Manage tags** page, you can view, create, remove and edit tags\. You can’t modify the AWS managed tags listed at the top\. However, you can add to and modify the customer managed tags with editing fields, listed after the AWS managed tags\.

   Choose **Add new tag** to create a new tag\.

1. Enter a key and value for the new tag\.

1. To edit a tag, enter a value in the tag value field for a selected key\.

1. To delete a tag choose **Remove** for a selected tag\.

1. When you have completed your changes, choose **Save changes**\.

### Create tags using the AWS Proton AWS CLI<a name="cli-tags"></a>

You can view, create, remove and edit tags using the AWS Proton AWS CLI\.

You can create or edit a tag for a resource as shown in the following example\.

```
aws proton tag-resource --resource-arn "arn:aws:proton:region-id:account-id:service-template/webservice" --tags '[{"key":"mykey1","value":"myval1"},{"key":"mykey2","value":"myval2"}]'
```

You can remove a tag for a resource as shown in the next example\.

```
aws proton untag-resource --resource-arn "arn:aws:proton:region-id:account-id:service-template/webservice" --tag-keys '["mykey1","mykey2"]'
```

You can list tags for a resource as shown in the final example\.

```
aws proton list-tags-for-resource --resource-arn "arn:aws:proton:region-id:account-id:service-template/webservice"
```