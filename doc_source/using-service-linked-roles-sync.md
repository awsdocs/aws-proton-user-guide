# Using roles for AWS Proton sync<a name="using-service-linked-roles-sync"></a>

AWS Proton uses AWS Identity and Access Management \(IAM\) [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to AWS Proton\. Service\-linked roles are predefined by AWS Proton and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up AWS Proton easier because you don’t have to manually add the necessary permissions\. AWS Proton defines the permissions of its service\-linked roles, and unless defined otherwise, only AWS Proton can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your AWS Proton resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-linked roles** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for AWS Proton<a name="service-linked-role-permissions-sync"></a>

AWS Proton uses two service\-linked roles named **AWSServiceRoleForProtonSync** and **AWSServiceRoleForProtonServiceSync**\.

The AWSServiceRoleForProtonSync service\-linked role trusts the following services to assume the role:
+ `sync.proton.amazonaws.com`

The role permissions policy named AWSProtonSyncServiceRolePolicy allows AWS Proton to complete the following actions on the specified resources:
+ Action: *create, manage, and read* on *AWS Proton templates and template versions*
+ Action: *use connection* on *AWS CodeStar connections*

### AWSProtonSyncServiceRolePolicy<a name="service-linked-role-permissions-sync.details"></a>

This policy includes the following permissions:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SyncToProton",
      "Effect": "Allow",
      "Action": [
        "proton:UpdateServiceTemplateVersion",
        "proton:UpdateServiceTemplate",
        "proton:UpdateEnvironmentTemplateVersion",
        "proton:UpdateEnvironmentTemplate",
        "proton:GetServiceTemplateVersion",
        "proton:GetServiceTemplate",
        "proton:GetEnvironmentTemplateVersion",
        "proton:GetEnvironmentTemplate",
        "proton:DeleteServiceTemplateVersion",
        "proton:DeleteEnvironmentTemplateVersion",
        "proton:CreateServiceTemplateVersion",
        "proton:CreateServiceTemplate",
        "proton:CreateEnvironmentTemplateVersion",
        "proton:CreateEnvironmentTemplate",
        "proton:ListEnvironmentTemplateVersions",
        "proton:ListServiceTemplateVersions",
        "proton:CreateEnvironmentTemplateMajorVersion",
        "proton:CreateServiceTemplateMajorVersion"
      ],
      "Resource": "*"
    },
    {
      "Sid": "AccessGitRepos",
      "Effect": "Allow",
      "Action": [
        "codestar-connections:UseConnection"
      ],
      "Resource": "arn:aws:codestar-connections:*:*:connection/*"
    }
  ]
}
```

For information on the the AWSProtonSyncServiceRolePolicy, see [AWS managed policy: AWSProtonSyncServiceRolePolicy](https://docs.aws.amazon.com/proton/latest/userguide/security-iam-awsmanpol.html#security-iam-awsmanpol-AWSProtonSyncServiceRolePolicy)\.

The AWSServiceRoleForProtonServiceSync service\-linked role trusts the following services to assume the role:
+ `service-sync.proton.amazonaws.com`

The role permissions policy named AWSServiceRoleForProtonServiceSync allows AWS Proton to complete the following actions on the specified resources:
+ Action: *create, manage, and read on AWS Proton services and service instances*

### AwsProtonServiceGitSyncServiceRolePolicy<a name="service-linked-role-permissions-AwsProtonServiceGitSyncServiceRolePolicy"></a>

This policy includes the following permissions:

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "ProtonServiceSync",
			"Effect": "Allow",
			"Action": [
				"proton:GetService",
				"proton:UpdateService",
				"proton:UpdateServicePipeline",
				"proton:CreateServiceInstance",
				"proton:GetServiceInstance",
				"proton:UpdateServiceInstance",
				"proton:ListServiceInstances",
				"proton:GetComponent",
				"proton:CreateComponent",
				"proton:ListComponents",
				"proton:UpdateComponent",
				"proton:GetEnvironment",
				"proton:CreateEnvironment",
				"proton:ListEnvironments",
				"proton:UpdateEnvironment"
			],
			"Resource": "*"
		}
	]
}
```

For information on the the AwsProtonServiceSyncServiceRolePolicy, see [AWS managed policy: AwsProtonServiceSyncServiceRolePolicy](https://docs.aws.amazon.com/proton/latest/userguide/security-iam-awsmanpol.html#security-iam-awsmanpol-AwsProtonServiceSyncServiceRolePolicy)\.

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for AWS Proton<a name="create-service-linked-role-sync"></a>

You don't need to manually create a service\-linked role\. When you configure a repository or service for sync in AWS Proton in the AWS Management Console, the AWS CLI, or the AWS API, AWS Proton creates the service\-linked role for you\. 

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you configure a repository or service for sync in AWS Proton, AWS Proton creates the service\-linked role for you again\. 

To recreate the AWSServiceRoleForProtonSync service\-linked role, you would want to configure a repository for sync, and to recreate AWSServiceRoleForProtonServiceSync, you would want to configure a service for sync\.

## Editing a service\-linked role for AWS Proton<a name="edit-service-linked-role-sync"></a>

AWS Proton doesn't allow you to edit the AWSServiceRoleForProtonSync service\-linked role\. After you create a service\-linked role, you can't change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for AWS Proton<a name="delete-service-linked-role-sync"></a>

You don't need to manually delete the AWSServiceRoleForProtonSync role\. When you delete all AWS Proton linked repositories for repository sync in the AWS Management Console, the AWS CLI, or the AWS API, AWS Proton cleans up the resources and deletes the service\-linked role for you\.

## Supported regions for AWS Proton service\-linked roles<a name="slr-regions-sync"></a>

AWS Proton supports using service\-linked roles in all of the AWS Regions where the service is available\. For more information, see [AWS Proton endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/proton.html) in the *AWS General Reference*\.