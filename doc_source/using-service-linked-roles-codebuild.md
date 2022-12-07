# Using roles for CodeBuild\-based provisioning<a name="using-service-linked-roles-codebuild"></a>

AWS Proton uses AWS Identity and Access Management \(IAM\) [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to AWS Proton\. Service\-linked roles are predefined by AWS Proton and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up AWS Proton easier because you don’t have to manually add the necessary permissions\. AWS Proton defines the permissions of its service\-linked roles, and unless defined otherwise, only AWS Proton can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your AWS Proton resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-linked roles** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for AWS Proton<a name="service-linked-role-permissions-codebuild"></a>

AWS Proton uses the service\-linked role named **AWSServiceRoleForProtonCodeBuildProvisioning** – A Service Linked Role for AWS Proton CodeBuild provisioning\.

The AWSServiceRoleForProtonCodeBuildProvisioning service\-linked role trusts the following services to assume the role:
+ `codebuild.proton.amazonaws.com`

The role permissions policy named AWSProtonCodeBuildProvisioningServiceRolePolicy allows AWS Proton to complete the following actions on the specified resources:
+ Action: *create, manage, and read* on *AWS CloudFormation stacks and transforms*
+ Action: *create, manage, and read* on *CodeBuild projects and builds*

### AWSProtonCodeBuildProvisioningServiceRolePolicy<a name="service-linked-role-permissions-codebuild.details"></a>

This policy includes the following permissions:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:CreateStack",
        "cloudformation:CreateChangeSet",
        "cloudformation:DeleteChangeSet",
        "cloudformation:DeleteStack",
        "cloudformation:DescribeStacks",
        "cloudformation:ListStackResources"
      ],
      "Resource": [
        "arn:aws:cloudformation:*:*:stack/AWSProton-CodeBuild-*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "codebuild:CreateProject",
        "codebuild:DeleteProject",
        "codebuild:StartBuild",
        "codebuild:StopBuild",
        "codebuild:RetryBuild",
        "codebuild:BatchGetBuilds",
        "codebuild:BatchGetProjects"
      ],
      "Resource": "arn:aws:codebuild:*:*:project/AWSProton*"
    },
    {
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "*",
      "Condition": {
        "StringEqualsIfExists": {
          "iam:PassedToService": "codebuild.amazonaws.com"
        }
      }
    }
  ]
}
```

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for AWS Proton<a name="create-service-linked-role-codebuild"></a>

You don't need to manually create a service\-linked role\. When you create an environment that uses CodeBuild\-based provisioning in AWS Proton in the AWS Management Console, the AWS CLI, or the AWS API, AWS Proton creates the service\-linked role for you\. 

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you create an environment that uses CodeBuild\-based provisioning in AWS Proton, AWS Proton creates the service\-linked role for you again\. 

## Editing a service\-linked role for AWS Proton<a name="edit-service-linked-role-codebuild"></a>

AWS Proton does not allow you to edit the AWSServiceRoleForProtonCodeBuildProvisioning service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for AWS Proton<a name="delete-service-linked-role-codebuild"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must  delete all environments and services \(instances and pipelines\) that use CodeBuild\-based provisioning in AWS Proton before you can manually delete it\.

### Manually delete the service\-linked role<a name="slr-manual-delete-codebuild"></a>

Use the IAM console, the AWS CLI, or the AWS API to delete the AWSServiceRoleForProtonCodeBuildProvisioning service\-linked role\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported regions for AWS Proton service\-linked roles<a name="slr-regions-codebuild"></a>

AWS Proton supports using service\-linked roles in all of the AWS Regions where the service is available\. For more information, see [AWS Proton endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/proton.html) in the *AWS General Reference*\.