# AWS Proton IAM service role policy examples<a name="security_iam_service-role-policy-examples"></a>

Administrators own and manage the resources that AWS Proton creates as defined by the environment and service templates\. They attach IAM service roles to their account that permit AWS Proton to create resources on their behalf\. Administrators supply the IAM roles and AWS Key Management Service keys for resources that are later owned and managed by developers when AWS Proton deploys their application as an AWS Proton service in an AWS Proton environment\. For more information about AWS KMS and data encryption, see [Data protection in AWS Proton](data-protection.md)\.

A service role is an Amazon Web Services \(IAM\) role that allows AWS Proton to make calls to resources on your behalf\. If you specify a service role, AWS Proton uses that role's credentials\. Use a service role to explicitly specify the actions that AWS Proton can perform\.

You create the service role and its permission policy with the IAM service\. For more information about creating a service role, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

## AWS Proton service role for provisioning using AWS CloudFormation<a name="proton-svc-role"></a>

As a member of the platform team, you can as an administrator create an AWS Proton service role and provide it to AWS Proton when you create an environment as the environment's CloudFormation service role \(the `protonServiceRoleArn` parameter of the [CreateEnvironment](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateEnvironment.html) API action\)\. This role allows AWS Proton to make API calls to other services on your behalf when the environment or any of the service instances running in it use AWS\-managed provisioning and AWS CloudFormation to provision infrastructure\.

We recommend that you use the following IAM role and trust policy for your AWS Proton service role\. When you use the AWS Proton console to create an environment and choose to create a new role, this is the policy that AWS Proton adds to the service role it creates for you\. When scoping down permission on this policy, keep in mind that AWS Proton fails on `Access Denied` errors\.

**Important**  
Be aware that the policies shown in the following examples grant administrator privileges to anyone that can register a template to your account\. Because we don't know which resources you will define in your AWS Proton templates, these policies have broad permissions\. We recommend that you scope down the permissions to the specific resources that will be deployed in your environments\.

### AWS Proton service role policy example for AWS CloudFormation<a name="proton-svc-role.details"></a>

Replace `123456789012` with your AWS account ID\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:CancelUpdateStack",
        "cloudformation:ContinueUpdateRollback",
        "cloudformation:CreateChangeSet",
        "cloudformation:CreateStack",
        "cloudformation:DeleteChangeSet",
        "cloudformation:DeleteStack",
        "cloudformation:DescribeChangeSet",
        "cloudformation:DescribeStackDriftDetectionStatus",
        "cloudformation:DescribeStackEvents",
        "cloudformation:DescribeStackResourceDrifts",
        "cloudformation:DescribeStacks",
        "cloudformation:DetectStackResourceDrift",
        "cloudformation:ExecuteChangeSet",
        "cloudformation:ListChangeSets",
        "cloudformation:ListStackResources",
        "cloudformation:UpdateStack"
      ],
      "Resource": "arn:aws:cloudformation:*:123456789012:stack/AWSProton-*"
    },
    {
      "Effect": "Allow",
      "NotAction": [
        "organizations:*",
        "account:*"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:CalledVia": [
            "cloudformation.amazonaws.com"
          ]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "organizations:DescribeOrganization",
        "account:ListRegions"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:CalledVia": [
            "cloudformation.amazonaws.com"
          ]
        }
      }
    }
  ]
}
```

### AWS Proton service trust policy<a name="proton-svc-role.trust.details"></a>

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "ServiceTrustRelationshipWithConfusedDeputyPrevention",
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

### Scoped down AWS\-managed provisioning service role policy<a name="proton-svc-role.scoped-down.details"></a>

The following is an example of a scoped down AWS Proton service role policy that you can use if you only need AWS Proton services to provision S3 resources\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:CancelUpdateStack",
        "cloudformation:ContinueUpdateRollback",
        "cloudformation:CreateChangeSet",
        "cloudformation:CreateStack",
        "cloudformation:DeleteChangeSet",
        "cloudformation:DeleteStack",
        "cloudformation:DescribeChangeSet",
        "cloudformation:DescribeStackDriftDetectionStatus",
        "cloudformation:DescribeStackEvents",
        "cloudformation:DescribeStackResourceDrifts",
        "cloudformation:DescribeStacks",
        "cloudformation:DetectStackResourceDrift",
        "cloudformation:ExecuteChangeSet",
        "cloudformation:ListChangeSets",
        "cloudformation:ListStackResources",
        "cloudformation:UpdateStack"
      ],
      "Resource": "arn:aws:cloudformation:*:123456789012:stack/AWSProton-*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:*"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:CalledVia": [
            "cloudformation.amazonaws.com"
          ]
        }
      }
    }
  ]
}
```

## AWS Proton service role for CodeBuild provisioning<a name="codebuild-proton-svc-role"></a>

As a member of the platform team, you can as an administrator create an AWS Proton service role and provide it to AWS Proton when you create an environment as the environment's CodeBuild service role \(the `codebuildRoleArn` parameter of the [CreateEnvironment](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateEnvironment.html) API action\)\. This role allows AWS Proton to make API calls to other services on your behalf when the environment or any of the service instances running in it use CodeBuild provisioning to provision infrastructure\.

When you use the AWS Proton console to create an environment and choose to create a new role, AWS Proton adds a policy with administrator privileges to the service role it creates for you\. When you create your own role and scope down permissions, keep in mind that AWS Proton fails on `Access Denied` errors\.

**Important**  
Be aware that the policies that AWS Proton attaches to roles that it creates for you grant administrator privileges to anyone that can register a template to your account\. Because we don't know which resources you will define in your AWS Proton templates, these policies have broad permissions\. We recommend that you scope down the permissions to the specific resources that will be deployed in your environments\.

### AWS Proton service role policy example for CodeBuild<a name="codebuild-proton-svc-role.details"></a>

The following example provides permissions for CodeBuild to provision resources using the AWS Cloud Development Kit \(AWS CDK\)\.

Replace `123456789012` with your AWS account ID\.

```
 {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "logs:CreateLogStream",
        "logs:CreateLogGroup",
        "logs:PutLogEvents"
      ],
      "Resource": [
        "arn:aws:logs:us-east-1:123456789012:log-group:/aws/codebuild/AWSProton-   Shell-*",
        "arn:aws:logs:us-east-1:123456789012:log-group:/aws/codebuild/AWSProton-   Shell-*:*"
      ],
      "Effect": "Allow"
    },
    {
      "Action": "proton:NotifyResourceDeploymentStatusChange",
      "Resource": "arn:aws:proton:us-east-1:123456789012:*",
      "Effect": "Allow"
    },
    {
      "Action": "sts:AssumeRole",
      "Resource": [
        "arn:aws:iam::123456789012:role/cdk-*-deploy-role-*",
        "arn:aws:iam::123456789012:role/cdk-*-file-publishing-role-*"
      ],
      "Effect": "Allow"
    }
  ]
}
```

### AWS Proton CodeBuild trust policy<a name="codebuild-proton-svc-role.trust.details"></a>

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "CodeBuildTrustRelationshipWithConfusedDeputyPrevention",
    "Effect": "Allow",
    "Principal": {
      "Service": "codebuild.amazonaws.com"
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

## AWS Proton pipeline service roles<a name="codepipeline-proton-svc-role"></a>

To provision service pipelines, AWS Proton needs permissions to make API calls to other services\. The required service roles are similar to the service roles you provide when you create environments\. However, the roles for creating pipelines are shared among all services in your AWS account, and you provide these roles as **Account settings** in the console, or through the [UpdateAccountSettings](https://docs.aws.amazon.com/proton/latest/APIReference/API_UpdateAccountSettings.html) API action\.

When you use the AWS Proton console to update account settings and choose to create a new role for either the AWS CloudFormation or the CodeBuild service roles, the policies that AWS Proton adds to the service roles it creates for you are the same as the policies described in the previous sections, [AWS\-managed provisioning role](#proton-svc-role) and [CodeBuild provisioning role](#codebuild-proton-svc-role)\. When scoping down permission on this policy, keep in mind that AWS Proton fails on `Access Denied` errors\.

**Important**  
Be aware that the example policies in the previous sections grant administrator privileges to anyone that can register a template to your account\. Because we don't know which resources you will define in your AWS Proton templates, these policies have broad permissions\. We recommend that you scope down the permissions to the specific resources that will be deployed in your pipelines\.

## AWS Proton component role<a name="proton-custom-comp-role"></a>

As a member of the platform team, you can as an administrator create an AWS Proton service role and provide it to AWS Proton when you create an environment as the environment's CloudFormation component role \(the `componentRoleArn` parameter of the [CreateEnvironment](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateEnvironment.html) API action\)\. This role scopes down the infrastructure that directly defined components can provision\. For more information about components, see [AWS Proton components](ag-components.md)\.

The following example policy supports creating a directly defined component that provisions an Amazon Simple Storage Service \(Amazon S3\) bucket and a related access policy\.

### AWS Proton component role policy example<a name="proton-custom-comp-role.details"></a>

Replace `123456789012` with your AWS account ID\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:CancelUpdateStack",
        "cloudformation:CreateChangeSet",
        "cloudformation:DeleteChangeSet",
        "cloudformation:DescribeStacks",
        "cloudformation:ContinueUpdateRollback",
        "cloudformation:DetectStackResourceDrift",
        "cloudformation:DescribeStackResourceDrifts",
        "cloudformation:DescribeStackEvents",
        "cloudformation:CreateStack",
        "cloudformation:DeleteStack",
        "cloudformation:UpdateStack",
        "cloudformation:DescribeChangeSet",
        "cloudformation:ExecuteChangeSet",
        "cloudformation:ListChangeSets",
        "cloudformation:ListStackResources"
      ],
      "Resource": "arn:aws:cloudformation:*:123456789012:stack/AWSProton-*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:CreateBucket",
        "s3:DeleteBucket",
        "s3:GetBucket",
        "iam:CreatePolicy",
        "iam:DeletePolicy",
        "iam:GetPolicy",
        "iam:ListPolicyVersions",
        "iam:DeletePolicyVersion"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringEquals": {
          "aws:CalledVia": "cloudformation.amazonaws.com"
        }
      }
    }
  ]
}
```

### AWS Proton component trust policy<a name="proton-custom-comp-role.trust.details"></a>

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "ServiceTrustRelationshipWithConfusedDeputyPrevention",
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