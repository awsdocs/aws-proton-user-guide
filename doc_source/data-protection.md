# Data protection in AWS Proton<a name="data-protection"></a>

AWS Proton conforms to the AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) which includes regulations and guidelines for data protection\. AWS is responsible for protecting the global infrastructure that runs all the AWS services\. AWS maintains control over data hosted on this infrastructure, including the security configuration controls for handling customer content and personal data\. AWS customers and APN partners, acting either as data controllers or data processors, are responsible for any personal data that they put in the AWS Cloud

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\), so that each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\. We recommend TLS 1\.2 or later\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.

We strongly recommend that you never put sensitive identifying information, such as your customers' account numbers, into free\-form text fields such as a **Name** field\. This includes when you work with AWS Proton or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into free form text fields for resource identifiers or similar items related to the management of AWS resources might get picked up for inclusion in diagnostic logs\. When you provide a URL to an external server, don't include credentials information in the URL to validate your request to that server\.

For more information about data protection, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog\.*

## Server side encryption at rest<a name="encrypt-at-rest"></a>

If you choose to encrypt sensitive data in your template bundles at rest in the S3 bucket where you store your template bundles, you must use an SSE\-S3 or SSE\-KMS key to allow AWS Proton to retrieve the template bundles so they can be attached to a registered AWS Proton template\.

## Encryption in transit<a name="encrypt-in-transit"></a>

All service to service communication is encrypted in transit using SSL/TLS\.

## AWS Proton encryption key management<a name="encryption-key-management"></a>

Within AWS Proton, all customer data is encrypted by default using an AWS Proton owned key\. If you supply a customer owned and managed AWS KMS key, all customer data is encrypted using the customer provided key as described in the following paragraphs\.

When you create an AWS Proton template, you specify your key and AWS Proton uses your credentials to create a grant which allows AWS Proton to use your key\.

If you manually retire the grant or, disable or delete your specified key, then AWS Proton is unable to read the data that was encrypted by the specified key and throws `ValidationException`\.

## AWS Proton encryption context<a name="encryption-context"></a>

AWS Proton supports encryption context headers\. An encryption context is an optional set of key\-value pairs that can contain additional contextual information about the data\. For general information about encryption context, see [AWS Key Management Service Concepts \- Encryption Context](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#encrypt_context) in the *AWS Key Management Service Developer Guide*\.

An encryption context is a set of key–value pairs that contain arbitrary non\-secret data\. When including an encryption context in a request to encrypt data, AWS KMS cryptographically binds the encryption context to the encrypted data\. To decrypt the data, you must pass in the same encryption context\.

 Customers can use the encryption context to identify use of their customer managed key in audit records and logs\. It also appears in plaintext in logs, such as AWS CloudTrail and Amazon CloudWatch Logs\.

AWS Proton does not take in any customer\-specified or externally\-specified encryption context\.

AWS Proton adds the following encryption context\.

```
{
  "aws:proton:template": "<proton-template-arn>",
  "aws:proton:resource": "<proton-resource-arn>" 
}
```

The first encryption context identifies the AWS Proton template that the resource is associated with and also serves as a constraint for customer managed key permissions and grants\.

The second encryption context identifies the AWS Proton resource that is encrypted\.

The following examples show AWS Proton encryption context use\.

Developer creating a service instance\.

```
{
  "aws:proton:template": "arn:aws:proton:region_id:123456789012:service-template/my-template",
  "aws:proton:resource": "arn:aws:proton:region_id:123456789012:service/my-service/service-instance/my-service-instance" 
}
```

An administrator creating a template\.

```
{
  "aws:proton:template": "arn:aws:proton:region_id:123456789012:service-template/my-template",
  "aws:proton:resource": "arn:aws:proton:region_id:123456789012:service-template/my-template"
}
```