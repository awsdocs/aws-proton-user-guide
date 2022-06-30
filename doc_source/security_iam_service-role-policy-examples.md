# AWS Proton IAM service role policy examples<a name="security_iam_service-role-policy-examples"></a>

Administrators own and manage the resources that AWS Proton creates as defined by the environment and service templates\. They attach IAM service roles to their account that permit AWS Proton to create resources on their behalf\. Administrators supply the IAM roles and AWS Key Management Service keys for resources that are later owned and managed by developers when AWS Proton deploys their application as an AWS Proton service in an AWS Proton environment\. For more information about AWS KMS and data encryption, see [Data protection in AWS Proton](data-protection.md)\.

A service role is an Amazon Web Services \(IAM\) role that allows AWS Proton to make calls to resources on your behalf\. If you specify a service role, AWS Proton uses that role's credentials\. Use a service role to explicitly specify the actions that AWS Proton can perform\.

You create the service role and its permission policy with the IAM service\. For more information about creating a service role, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

## AWS Proton service role<a name="proton-svc-role"></a>

As a member of the platform team, you can as an administrator create an AWS Proton service role to allow AWS Proton to make API calls to other services, like CloudFormation, on your behalf\.

We recommend that you use the following IAM role and trust policy for your AWS Proton service role\. When you use the AWS Proton console to create your roles, this is the AWS Proton service role policy that AWS Proton creates for you\. When scoping down permission on this policy, keep in mind that AWS Proton fails on `Access Denied` errors\.

**Important**  
Be aware that the policies shown in the following examples grant administrator privileges to anyone that can register a template to your account\. Because we don't know which resources you will define in your AWS Proton templates, these policies have broad permissions\. We recommend that you scope down the permissions to the specific resources that will be deployed in your environments\.

**IAM AWS Proton service role policy**

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
                    "aws:CalledVia": ["cloudformation.amazonaws.com"]
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
                    "aws:CalledVia": ["cloudformation.amazonaws.com"]
                }
            }
        }
    ]
}
```

**IAM AWS Proton service trust policy**

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Sid": "ServiceTrustRelationshipWithConfusedDeputyPrevention",
        "Effect": "Allow",
        "Principal": {"Service": "proton.amazonaws.com"},
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
            "Action": ["s3:*"],
            "Resource": "*",
            "Condition": {
                "ForAnyValue:StringEquals": {
                    "aws:CalledVia": ["cloudformation.amazonaws.com"]
                }
            }
        }
    ]
}
```

## AWS Proton pipeline service role<a name="codepipeline-proton-svc-role"></a>

As a member of the platform team, you, as an administrator, can create an AWS Proton pipeline service role to allow AWS Proton to make CloudFormation API calls to deploy a pipeline CloudFormation stack on your behalf\.

We recommend that you use the following IAM role and trust policy for your AWS Proton pipeline service role\. When you use the AWS Proton console to create your roles, this is the AWS Proton pipeline service role policy that AWS Proton creates for you\. When scoping down permission on this policy, keep in mind that AWS Proton fails on `Access Denied` errors\.

**Important**  
Be aware that the policies shown in the following examples grant administrator privileges to anyone that can register a template to your account\. Because we don't know which resources you will define in your AWS Proton templates, these policies have broad permissions\. We recommend that you scope down the permissions to the specific resources that will be deployed in your pipelines\.

**IAM AWS Proton pipeline service role policy**

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
                    "aws:CalledVia": ["cloudformation.amazonaws.com"]
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
                    "aws:CalledVia": ["cloudformation.amazonaws.com"]
                }
            }
        }
    ]
}
```

**IAM AWS Proton pipeline service trust policy**

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Sid": "PipelineTrustRelationshipWithConfusedDeputyPrevention",
        "Effect": "Allow",
        "Principal": {"Service": "proton.amazonaws.com"},
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

To see an example of a scoped down policy, see [AWS Proton service role](#proton-svc-role)\.

## AWS Proton component role<a name="proton-custom-comp-role"></a>

As a member of the platform team, you can as an administrator create an AWS Proton service role to allow AWS Proton to provision directly defined components on your behalf\. This role scopes down the infrastructure that directly defined components can provision\. For more information about components, see [AWS Proton components](ag-components.md)\.

The following example policy supports creating a directly defined component that provisions an Amazon Simple Storage Service \(Amazon S3\) bucket and a related access policy\.

**IAM AWS Proton directly defined component policy example**

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

**IAM AWS Proton service trust policy**

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Sid": "ServiceTrustRelationshipWithConfusedDeputyPrevention",
        "Effect": "Allow",
        "Principal": {"Service": "proton.amazonaws.com"},
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