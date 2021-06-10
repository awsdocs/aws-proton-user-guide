# Security best practices for AWS Proton<a name="security-best-practices"></a>

AWS Proton provides security features to consider as you develop and implement your own security policies\. The following best practices are general guidelines and don’t represent a complete security solution\. Because these best practices might not be appropriate or sufficient for your environment, treat them as helpful considerations rather than prescriptions\. 

**Topics**
+ [Use IAM to control access](#use-iam-to-control-access)
+ [Do not embed credentials in your templates and template bundles](#creds)
+ [Use encryption to protect sensitive data](#encryption)
+ [Use AWS CloudTrail to view and log API calls](#cloudtrail)

## Use IAM to control access<a name="use-iam-to-control-access"></a>

IAM is an AWS service that you can use to manage users and their permissions in AWS\. You can use IAM with AWS Proton to specify which AWS Proton actions administrators and developers can perform, such as managing templates, environments or services\. You can use IAM service roles to allow AWS Proton to make calls to other services on your behalf\.

For more information on AWS Proton and IAM roles, see [Identity and Access Management for AWS Proton](security-iam.md)\.

Implement least privilege access\. For more information, see [Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *AWS Identity and Access Management User Guide*\.

## Do not embed credentials in your templates and template bundles<a name="creds"></a>

Rather than embedding sensitive information in your AWS CloudFormation templates and template bundles, we recommend you use *dynamic references* in your stack template\.

Dynamic references provide a compact, powerful way for you to reference external values that are stored and managed in other services, such as the AWS Systems Manager Parameter Store or AWS Secrets Manager\. When you use a dynamic reference, CloudFormation retrieves the value of the specified reference when necessary during stack and change set operations, and passes the value to the appropriate resource\. However, CloudFormation never stores the actual reference value\. For more information, see [Using Dynamic References to Specify Template Values](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/dynamic-references.html) in the *AWS CloudFormation User Guide*\.

[AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) helps you to securely encrypt, store, and retrieve credentials for your databases and other services\. The [AWS Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html) provides secure, hierarchical storage for configuration data management\. 

For more information on defining template parameters, see [https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html) in the *AWS CloudFormation User Guide*\.

## Use encryption to protect sensitive data<a name="encryption"></a>

Within AWS Proton, all customer data is encrypted by default using an AWS Proton owned key\.

As a member of the platform team, you can provide a customer managed key to AWS Proton to encrypt and secure your sensitive data\. Encrypt sensitive data at rest in your S3 bucket\. For more information, see [Data protection in AWS Proton](data-protection.md)\.

## Use AWS CloudTrail to view and log API calls<a name="cloudtrail"></a>

AWS CloudTrail tracks anyone making API calls in your AWS account\. API calls are logged whenever anyone uses the AWS Proton API, the AWS Proton console or AWS Proton AWS CLI commands\. Enable logging and specify an Amazon S3 bucket to store the logs\. That way, if you need to, you can audit who made what AWS Proton call in your account\. For more information, see [Logging and monitoring in AWS Proton](security-logging-and-monitoring.md)\.