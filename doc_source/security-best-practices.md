--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# Security Best Practices for AWS Proton<a name="security-best-practices"></a>

AWS Proton provides security features to consider as you develop and implement your own security policies\. The following best practices are general guidelines and don’t represent a complete security solution\. Because these best practices might not be appropriate or sufficient for your environment, treat them as helpful considerations rather than prescriptions\. 

**Topics**
+ [Use IAM to control access](#use-iam-to-control-access)
+ [Do not embed credentials in your templates and template bundles](#creds)
+ [Use encryption to protect sensitive data](#encryption)
+ [Use AWS CloudTrail to view and log API calls](#cloudtrail)

## Use IAM to control access<a name="use-iam-to-control-access"></a>

IAM is an AWS service that you can use to manage users and their permissions in AWS\. You can use IAM with Proton to specify which Proton actions administrators and developers can perform, such as managing templates, environments or services\. You can use IAM service roles to allow Proton to make calls to other services on your behalf\.

For more information on Proton and IAM roles, see [Controlling access with IAM](ag-controlling-access.md)\.

Implement least privilege access\. For more information, see [Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *AWS Identity and Access Management User Guide*\.

## Do not embed credentials in your templates and template bundles<a name="creds"></a>

Rather than embedding sensitive information in your AWS CloudFormation templates and template bundles, we recommend you use *dynamic references* in your stack template\.

Dynamic references provide a compact, powerful way for you to reference external values that are stored and managed in other services, such as the AWS Systems Manager Parameter Store or AWS Secrets Manager\. When you use a dynamic reference, CloudFormation retrieves the value of the specified reference when necessary during stack and change set operations, and passes the value to the appropriate resource\. However, CloudFormation never stores the actual reference value\. For more information, see [Using Dynamic References to Specify Template Values](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/dynamic-references.html) in the *AWS CloudFormation User Guide*\.

[AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) helps you to securely encrypt, store, and retrieve credentials for your databases and other services\. The [AWS Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html) provides secure, hierarchical storage for configuration data management\. 

For more information on defining template parameters, see [https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html) in the *AWS CloudFormation User Guide*\.

## Use encryption to protect sensitive data<a name="encryption"></a>

Within Proton, all customer data is encrypted by default using a Proton owned key\.

As a member of the platform team, you can provide a customer managed key to Proton to encrypt and secure your sensitive data\. Encrypt sensitive data at rest in your S3 bucket\. For more information, see [Data Protection in AWS Proton](data-protection.md)\.

## Use AWS CloudTrail to view and log API calls<a name="cloudtrail"></a>

AWS CloudTrail tracks anyone making API calls in your AWS account\. API calls are logged whenever anyone uses the Proton API, the Proton console or Proton AWS CLI commands\. Enable logging and specify an Amazon S3 bucket to store the logs\. That way, if you need to, you can audit who made what Proton call in your account\. For more information, see [Logging and monitoring](security-logging-and-monitoring.md)\.