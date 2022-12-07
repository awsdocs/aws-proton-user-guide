# CodeBuild provisioning role creation<a name="ag-env-codebuild-provisioning-role-creation"></a>

Infrastructure as a Code \(IaaC\) tools like AWS CloudFormation and Terraform require permissions for the many different types of AWS resources\. For example, if an IaaC template declares an Amazon S3 bucket, it needs permissions to create, read, update, and delete Amazon S3 buckets\. It's considered a security best practice to limit roles to the minimal permissions required\. Given the breadth of AWS resources, it’s challenging to create least\-privilege policies for IaaC templates, especially when the resources being managed by those templates can change later\. For example, in your latest edits to a template being managed by AWS Proton, you add an RDS database resource\.

Configuring the right permissions helps make deployments of your IaC smooth\. AWS Proton CodeBuild Provisioning executes arbitrary customer\-supplied CLI commands in a CodeBuild project located in the customer’s account\. Typically, these commands create and delete infrastructure using an Infrastructure as Code \(IaaC\) tool such as AWS CDK\. When an AWS resource deploys whose template uses CodeBuild Provisioning, AWS will start a build in a CodeBuild project managed by AWS\. A role is passed to CodeBuild, which CodeBuild assumes to execute commands\. This role, called the CodeBuild Provisioning Role, is provided by the customer and contains permissions required to provision infrastructure\. It's meant to be assumed only by CodeBuild and even AWS Proton can't assume it\.

**Creating the role**  
The CodeBuild Provisioning role can be created in the IAM console or in the AWS CLI\. To create it in the AWS CLI:

```
aws iam create-role --role-name AWSProtonCodeBuildProvisioning --assume-role-policy-document '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Principal":{"Service":"codebuild.amazonaws.com"},"Action":"sts:AssumeRole"}]}'
aws iam attach-role-policy --role-name AWSProtonCodeBuildProvisioning --policy-arn arn:aws:iam::aws:policy/AWSProtonCodeBuildProvisioningBasicAccess
```

This also attaches the `AWSProtonCodeBuildProvisioningBasicAccess`, which contains the minimal permissions needed by the CodeBuild service to run a build\.

If you prefer to use the console, please ensure the following when you create the role:

1. For trusted entity, select AWS service and then select CodeBuild\.

1. In the Add permissions step, select `AWSProtonCodeBuildProvisioningBasicAccess` and any other policies you want to attach\.

**Administrator Access**  
If you attach the `AdministratorAccess` policy to the CodeBuild Provisioning Role, it will guarantee that any IaaC template won't fail due to lack of permissions\. It also means that anyone who can create an Environment Template or Service Template can perform administrator\-level actions, even if that user isn't an administrator\. AWS Proton doesn't recommend using `AdministatorAccess` with the CodeBuild Provisioning Role\. If you decide to use `AdministratorAccess` with the CodeBuild Provisioning Role, do so in a sandbox environment\.

You can create a role with `AdministratorAccess` in the IAM console or by executing this command:

```
aws iam create-role --role-name AWSProtonCodeBuildProvisioning --assume-role-policy-document '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Principal":{"Service":"codebuild.amazonaws.com"},"Action":"sts:AssumeRole"}]}'
aws iam attach-role-policy --role-name AWSProtonCodeBuildProvisioning --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

**Creating a Minimally\-Scoped Role**

If you want to create a role with minimum permissions, there are multiple approaches:
+ Deploy with admin permissions, then scope down the role\. We recommend using [IAM Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html)\.
+ Use managed policies to give access to the services you plan on using\.

**AWS CDK**  
If you're using AWS CDK with AWS Proton, and you’ve run `cdk bootstrap` on each environment account/Region, then there already exists a role for `cdk deploy`\. In this case, attach the following policy to the CodeBuild Provisioning Role:

```
{
    "Action": "sts:AssumeRole",
    "Resource": [
        "arn:aws:iam::account-id:role/cdk-*-deploy-role-*",
        "arn:aws:iam::account-id:role/cdk-*-file-publishing-role-*"
    ],
    "Effect": "Allow"
}
```

**Custom VPC**  
If you decide to run CodeBuild in a [custom VPC](https://docs.aws.amazon.com/proton/latest/userguide/vpc-codebuild-custom-support.html), you’ll need the following permissions in your CodeBuild role:

```
{
    "Effect": "Allow",
    "Action": [
        "ec2:CreateNetworkInterface"
    ],
    "Resource": [
        "arn:aws:ec2:region:account-id:network-interface/*",
        "arn:aws:ec2:region:account-id:subnet/*",
        "arn:aws:ec2:region:account-id:security-group/*"
    ]
},
{
    "Effect": "Allow",
    "Action": [
        "ec2:DeleteNetworkInterface"
    ],
    "Resource": [
        "arn:aws:ec2:region:account-id:*/*"
    ]
},
{
    "Effect": "Allow",
    "Action": [
        "ec2:DescribeDhcpOptions",
        "ec2:DescribeNetworkInterfaces",
        "ec2:DescribeSubnets",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeVpcs"
    ],
    "Resource": "*"
},
{
    "Effect": "Allow",
    "Action": [
        "ec2:CreateNetworkInterfacePermission"
    ],
    "Resource": "arn:aws:ec2:region:account-id:network-interface/*",
    "Condition": {
        "StringEquals": {
            "ec2:AuthorizedService": "codebuild.amazonaws.com"
        }
    }
}
```

You could also use the `[AmazonEC2FullAccess](https://us-east-1.console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AmazonEC2FullAccess)` managed policy, although that includes permissions that you may not need\. To attach the managed policy using the CLI:

```
aws iam create-role --role-name AWSProtonCodeBuildProvisioning --assume-role-policy-document '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Principal":{"Service":"codebuild.amazonaws.com"},"Action":"sts:AssumeRole"}]}'
aws iam attach-role-policy --role-name AWSProtonCodeBuildProvisioning --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```