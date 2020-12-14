--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# Controlling access with IAM<a name="ag-controlling-access"></a>

With AWS Identity and Access Management \(IAM\), you can create IAM users to control who has access to which resources in your AWS account\. You can use IAM with AWS Proton to control what users can do with AWS Proton, such as whether they can create service templates or service instances or whether Proton can make API calls to other services on their behalf\.

**Note**  
For Public Preview, administrators, developers and environments must be under the same account\.

Administrators own and manage the resources that Proton creates as defined by the environment and service templates\. They attach IAM service roles to their account that permit Proton to create resources on their behalf\. Administrators supply the IAM roles and AWS KMS keys for resources that are later owned and managed by developers when Proton deploys their application as a Proton service in a Proton environment\. For more information on KMS and data encryption, see [Data Protection in AWS Proton](data-protection.md)\.

A service role is an AWS Identity and Access Management \(IAM\) role that allows AWS Proton to make calls to resources on your behalf\. If you specify a service role, AWS Proton uses that role's credentials\. Use a service role to explicitly specify the actions that AWS Proton can perform\.

You create the service role and its permission policy with the IAM service\. For more information about creating a service role, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

## AWS Proton service role<a name="arrow-svc-role"></a>

As members of the platform team, administrators create a Proton service role to allow Proton to make API calls to other services, like CloudFormation, on their behalf\.

We recommend that you use the following IAM role and trust policy for your Proton service role\. When you use the Proton console to create your roles, this is the Proton service role policy that Proton creates for you\. When scoping down permission on this policy, keep in mind that Proton fails on `Access Denied` errors\.

**Important**  
Be aware that the policies shown in the following examples grant administrator privileges to anyone that can register a template to your account\. Because we don't know which resources you will define in your Proton templates, these policies have broad permissions\. We recommend that you scope down the permissions to the specific resources that will be deployed in your environments\.

**IAM AWS Proton service role policy**

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "NotAction": [
        "organizations:*",
        "account:*"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "organizations:DescribeOrganization",
        "account:ListRegions"
      ],
      "Resource": "*"
    },
    {
        "Effect": "Allow",
        "Action": [
            "cloudformation:CancelUpdateStack",
            "cloudformation:ContinueUpdateRollback",
            "cloudformation:CreateChangeSet",
            "cloudformation:CreateStack",
            "cloudformation:DeleteChangeSet",
            "cloudformation:DeleteStack",
            "cloudformation:DescribeStackEvents",
            "cloudformation:DescribeChangeSet",
            "cloudformation:DescribeStackDriftDetectionStatus",
            "cloudformation:DescribeStackResourceDrifts",
            "cloudformation:DetectStackResourceDrift",
            "cloudformation:ExecuteChangeSet",
            "cloudformation:ListChangeSets",
            "cloudformation:ListStackResources",
            "cloudformation:UpdateStack"
        ],
        "Resource": "arn:aws:cloudformation:*:{{account_id}}:stack/AWSProton-*"
    }
  ]
}
```

**IAM AWS Proton service trust policy**

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Principal": {"Service": "proton.amazonaws.com"},
        "Action": "sts:AssumeRole"
    }
}
```

The following is an example of a scoped down Proton service role policy that you can use if you only need Proton services to provision S3 resources\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:*"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:CancelUpdateStack",
        "cloudformation:ContinueUpdateRollback",
        "cloudformation:CreateChangeSet",
        "cloudformation:CreateStack",
        "cloudformation:DeleteChangeSet",
        "cloudformation:DeleteStack",
        "cloudformation:DescribeStackEvents",
        "cloudformation:DescribeChangeSet",
        "cloudformation:DescribeStackDriftDetectionStatus",
        "cloudformation:DescribeStackResourceDrifts",
        "cloudformation:DetectStackResourceDrift",
        "cloudformation:ExecuteChangeSet",
        "cloudformation:ListChangeSets",
        "cloudformation:ListStackResources",
        "cloudformation:UpdateStack"
      ],
      "Resource": "arn:aws:cloudformation:*:{{account_id}}:stack/AWSProton-*"
    }
  ]
}
```

## AWS Proton pipeline service role<a name="codepipeline-arrow-svc-role"></a>

As members of the platform team, administrators create an AWS Proton pipeline service role to allow AWS Proton to make CloudFormation API calls to deploy a pipeline CloudFormation stack on their behalf\.

We recommend that you use the following IAM role and trust policy for your Proton pipeline service role\. When you use the Proton console to create your roles, this is the Proton pipeline service role policy that Proton creates for you\. When scoping down permission on this policy, keep in mind that Proton fails on `Access Denied` errors\.

**Important**  
Be aware that the policies shown in the following examples grant administrator privileges to anyone that can register a template to your account\. Because we don't know which resources you will define in your Proton templates, these policies have broad permissions\. We recommend that you scope down the permissions to the specific resources that will be deployed in your pipelines\.

**IAM Proton pipeline service role policy**

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "NotAction": [
        "organizations:*",
        "account:*"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "organizations:DescribeOrganization",
        "account:ListRegions"
      ],
      "Resource": "*"
    },
    {
        "Effect": "Allow",
        "Action": [
            "cloudformation:CancelUpdateStack",
            "cloudformation:ContinueUpdateRollback",
            "cloudformation:CreateChangeSet",
            "cloudformation:CreateStack",
            "cloudformation:DeleteChangeSet",
            "cloudformation:DeleteStack",
            "cloudformation:DescribeStackEvents",
            "cloudformation:DescribeChangeSet",
            "cloudformation:DescribeStackDriftDetectionStatus",
            "cloudformation:DescribeStackResourceDrifts",
            "cloudformation:DetectStackResourceDrift",
            "cloudformation:ExecuteChangeSet",
            "cloudformation:ListChangeSets",
            "cloudformation:ListStackResources",
            "cloudformation:UpdateStack"
        ],
        "Resource": "arn:aws:cloudformation:*:{{account_id}}:stack/AWSProton-*"
    }
  ]
}
```

**IAM Proton pipeline service trust policy**

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Principal": {"Service": "proton.amazonaws.com"},
        "Action": "sts:AssumeRole"
    }
}
```

To see an example of a scoped down policy, see [AWS Proton service role](#arrow-svc-role)\.

## Developer AWS Proton access roles<a name="env-dev-permissions"></a>

As members of the platform team, administrators create developer access permissions\. At the start, developers need only to be granted permission to make Proton API calls as shown in the IAM policy example below\. We recommend that permissions be scoped down to grant [least privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "proton:ListServiceTemplates",
                "proton:ListServiceTemplateMajorVersions",
                "proton:ListServiceTemplateMinorVersions",
                "proton:ListServices",
                "proton:ListServiceInstances",
                "proton:ListEnvironments",
                "proton:GetServiceTemplate",
                "proton:GetServiceTemplateMajorVersion",
                "proton:GetServiceTemplateMinorVersion",
                "proton:GetService",
                "proton:GetServiceInstance",
                "proton:GetEnvironment",
                "proton:CreateService",
                "proton:UpdateService",
                "proton:UpdateServiceInstance",
                "proton:UpdateServicePipeline",
                "proton:DeleteService"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "codestar-connections:PassConnection",
                "codestar-connections:ListConnections"
            ],
            "Resource": "*"
        }
    ]
}
```