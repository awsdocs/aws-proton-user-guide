# AWS managed policies for AWS Proton<a name="security-iam-awsmanpol"></a>

To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

AWS Proton provides managed IAM policies and trust relationships that you can attach to IAM users, groups, or roles that allow differing levels of control over resources and API operations\. You can apply these policies directly, or you can use them as starting points for creating your own policies\.

The following trust relationship is used for each of the AWS Proton managed policies\.

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "ExampleTrustRelationshipWithProtonConfusedDeputyPrevention",
    "Effect": "Allow",
    "Principal": {
      "Service": "proton.amazonaws.com"
    },
    "Action": "sts:AssumeRole",
    "Condition": {
      "StringEquals": {
        "aws:SourceAccount": "123456789012"
      },
      "ArnLike": {
        "aws:SourceArn": "arn:aws::proton:*:123456789012:environment/*"
      }
    }
  }
}
```

## AWS managed policy: AWSProtonFullAccess<a name="security-iam-awsmanpol-AWSProtonFullAccess"></a>

You can attach AWSProtonFullAccess to your IAM entities\. AWS Proton also attaches this policy to a service role that allows AWS Proton to perform actions on your behalf\. 

This policy grants administrative permissions that allow full access to AWS Proton and limited access to AWS Key Management Service, IAM and AWS CodeStar connections services\.

**Permissions details**

This policy includes the following permissions\.

This managed policy provides administrative access to the AWS Proton APIs and AWS Management Console\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "proton:*",
        "codestar-connections:ListConnections",
        "kms:ListAliases",
        "kms:DescribeKey"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "kms:CreateGrant"
      ],
      "Resource": "*",
      "Condition": {
        "StringLike": {
          "kms:ViaService": "proton.*.amazonaws.com"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "iam:PassedToService": "proton.amazonaws.com"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "codestar-connections:PassConnection"
      ],
      "Resource": "arn:aws:codestar-connections:*:*:connection/*",
      "Condition": {
        "StringEquals": {
          "codestar-connections:PassedToService": "proton.amazonaws.com"
        }
      }
    }
  ]
}
```
+ `proton` – Allows administrators full access to AWS Proton APIs\.
+ `iam` – Allows administrators to pass roles to AWS Proton\. This is required so that AWS Proton can make API calls to other services on the administrator's behalf\.
+ `kms` – Allows administrators to add a grant to a customer managed key\.
+ `codestar-connections` – Allows administrators to list and pass codestar\-connections so they can be used by AWS Proton\.

## AWS managed policy: AWSProtonReadOnlyAccess<a name="security-iam-awsmanpol-AWSProtonReadOnlyAccess"></a>

You can attach AWSProtonReadOnlyAccess to your IAM entities\. AWS Proton also attaches this policy to a service role that allows AWS Proton to perform actions on your behalf\. 

This policy grants read\-only permissions that allow read only access to AWS Proton APIs\.

**Permissions details**

This policy includes the following permissions\.

This managed policy provides read only access to the AWS Proton APIs and AWS Management Console\.

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": [
      "proton:List*",
      "proton:Get*"
    ],
    "Resource": "*"
  }
}
```
+ `proton` – Allows contributors read\-only access to AWS Proton APIs\.

## AWS managed policy: AWSProtonSyncServiceRolePolicy<a name="security-iam-awsmanpol-AWSProtonSyncServiceRolePolicy"></a>

AWS Proton attaches this policy to a service linked role that allows AWS Proton sync to perform actions on your behalf\.

This policy grants permissions that allow limited access to AWS Proton and AWS CodeStar connections APIs\.

**Permissions details**

This policy includes the following permissions\.

This managed policy provides limited access to the AWS Proton APIs and AWS Management Console\.

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
+ `proton` – Allows AWS Proton sync limited access to AWS Proton APIs\.
+ `proton` – Allows AWS Proton sync limited access to AWS CodeStar connections APIs\.

## AWS Proton updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>

View details about updates to AWS managed policies for AWS Proton since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the AWS Proton Document history page\.


| Change | Description | Date | 
| --- | --- | --- | 
|  [AWSProtonSyncServiceRolePolicy](#security-iam-awsmanpol-AWSProtonSyncServiceRolePolicy) – provides limited access to AWS Proton API operations and AWS Management Console\.  |  AWS Proton added a new policy to provide limited access to the AWS Proton API operations and AWS Management Console\.  | NOVEMBER 23, 2021 | 
|  [AWSProtonFullAccess](#security-iam-awsmanpol-AWSProtonFullAccess) – provides administrative access to the AWS Proton API operationsAPI operations and AWS Management Console\.  |  AWS Proton added a new policy to provide administrative access to the AWS Proton API operations and AWS Management Console\.  | JUNE 09, 2021 | 
|  [AWSProtonReadOnlyAccess](#security-iam-awsmanpol-AWSProtonReadOnlyAccess) – added a new policy\.  |  AWS Proton added a new policy to allow read\-only access to AWS Proton API operations\.  | JUNE 09, 2021 | 
|  AWS Proton started tracking changes\.  |  AWS Proton started tracking changes for its AWS managed policies\.  | JUNE 09, 2021 | 