# Using service\-linked roles for AWS Proton<a name="using-service-linked-roles"></a>

AWS Proton uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that's linked directly to AWS Proton\. Service\-linked roles are predefined by AWS Proton and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up AWS Proton easier because you don’t have to manually add the necessary permissions\. AWS Proton defines the permissions of its service\-linked roles, and unless defined otherwise, only AWS Proton can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

For information about other services that support service\-linked roles, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for AWS Proton<a name="slr-permissions"></a>

AWS Proton uses the service\-linked role named **AWSServiceRoleForProtonSync**\.

The AWSServiceRoleForProtonSync service\-linked role trusts the following services to assume the role:
+ `sync.proton.amazonaws.com`

The role permissions policy named AWSProtonSyncServiceRolePolicy allows AWS Proton to complete the following actions on the specified resources:
+ Action: *create, manage, and read* on *AWS Proton templates and template versions*
+ Action: *use connection* on *AWS CodeStar connections*

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for AWS Proton<a name="create-slr"></a>

You don't need to manually create a service\-linked role\. When you create a repository in AWS Proton using the AWS CLI, or the AWS API, AWS Proton creates the service\-linked role for you\. 

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you create a repository in AWS Proton, AWS Proton creates the service\-linked role for you again\. 

## Editing a service\-linked role for AWS Proton<a name="edit-slr"></a>

AWS Proton doesn't allow you to edit the AWSServiceRoleForProtonSync service\-linked role\. After you create a service\-linked role, you can't change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for AWS Proton<a name="delete-slr"></a>

You don't need to manually delete the AWSServiceRoleForProtonSync role\. When you delete all AWS Proton linked repositories for repository sync in the AWS Management Console, the AWS CLI, or the AWS API, AWS Proton cleans up the resources and deletes the service\-linked role for you\. For more information, see [Delete a template sync configuration](delete-template-sync.md) and [Delete a repository link](ag-repo-delete.md)\.

You can also use the IAM console, the AWS CLI or the AWS API to manually delete the service\-linked role\. To do this, you must first manually clean up the resources for your service\-linked role and then you can manually delete it\.

**Note**  
If the AWS Proton service is using the role when you try to delete the resources, then the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

**To manually delete the service\-linked role using IAM**

Use the IAM console, the AWS CLI, or the AWS API to delete the AWSServiceRoleForProtonSync service\-linked role\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported AWS Regions for AWS Proton service\-linked roles<a name="slr-regions"></a>

AWS Proton supports using service\-linked roles in all of the AWS Regions where the service is available\. For more information, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/proton.html)\.