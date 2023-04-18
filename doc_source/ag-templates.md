# AWS Proton templates<a name="ag-templates"></a>

To add your template bundle to your AWS Proton template library, create a template minor version and register it with AWS Proton\. When creating the template, provide the name of the Amazon S3 bucket and path for your template bundle\. After templates are published, they can be selected by platform team members and developers\. After they're selected, AWS Proton uses the template to create and provision infrastructure and applications\.

As an administrator, you can create and register an environment template with AWS Proton\. This environment template can then be used to deploy multiple environments\. For example, it can be used to deploy "dev," "staging," and "prod" environments\. The "dev" environment might include a VPC with private subnets and a restrictive access policy to all resources\. Environment outputs can be used as inputs for services\.

You can create and register environment templates to create two different types of environments\. Both you and developers can use AWS Proton to deploy services to both types\.
+ Register and publish a *standard* environment template that AWS Proton uses to create a *standard* environment that provisions and manages the environment infrastructure\.
+ Register and publish a *customer managed* environment template that AWS Proton uses to create a customer managed environment that connects to your existing provisioned infrastructure\. AWS Proton *doesn't* manage your existing provisioned infrastructure\.

You can create and register service templates with AWS Proton to deploy services to environments\. An AWS Proton environment must be created before a service can be deployed to it\.

The following list describes how you create and manage templates with AWS Proton\.
+ \(Optional\) Prepare an IAM role to control developer access to AWS Proton API calls and AWS Proton IAM service roles\. For more information, see [IAM Roles](ag-environment-roles.md)\.
+ Compose a template bundle\. For more information, see [Template bundles](ag-template-authoring.md#ag-template-bundles)\.
+ Create and register a template with AWS Proton after the template bundle is composed, compressed, and saved in an Amazon S3 bucket\. You can do this either in the console or by using the AWS CLI\.
+ Test and use the template to create and manage AWS Proton provisioned resources after it's registered with AWS Proton\.
+ Create and manage major and minor versions of the template throughout the life of the template\.

You can manage template versions manually or with template sync configurations:
+ Use the AWS Proton console and AWS CLI to create a new minor or major version\.
+ [Create a template sync configuration](create-template-sync.md) that lets AWS Proton automatically create a new minor or major version when it detects a change to your template bundle in a repository that you define\.

For additional information, see the [https://docs.aws.amazon.com/proton/latest/APIReference/Welcome.html](https://docs.aws.amazon.com/proton/latest/APIReference/Welcome.html)\.

**Topics**
+ [Versioned templates](ag-template-versions.md)
+ [Register and publish templates](template-create.md)
+ [View template data](template-view.md)
+ [Update a template](template-update.md)
+ [Delete templates](template-delete.md)
+ [Template sync configurations](ag-template-sync-configs.md)
+ [Service sync configurations](ag-service-sync-configs.md)