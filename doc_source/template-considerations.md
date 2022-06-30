# Template bundle considerations<a name="template-considerations"></a>


+ **Infrastructure as code \(IaC\) files**

  AWS Proton audits templates for the correct file format\. However, AWS Proton doesn't check for template development, dependency, and logic errors\. For example, assume that you specified the creation of an Amazon S3 bucket in your AWS CloudFormation IaC file as part of your service or environment template\. A service is created based on those templates\. Now, suppose at some point you want to delete the service\. If the specified S3 bucket *isn't* empty and the CloudFormation IaC file *doesn't* mark it as `Retain` in the `DeletionPolicy`, AWS Proton fails on the service delete operation\.
+ **Bundle file size limits and format**
  + Bundle file size, count, and name size limits can be found at [AWS Proton quotas](ag-limits.md)\.
  + The template bundle directories of files are gzipped into a tar ball and located in an Amazon Simple Storage Service \(Amazon S3\) bucket\.
  + Each file in the bundle must be a valid formatted YAML file\.
+ **S3 bucket template bundle encryption**

  If you want to encrypt sensitive data in your template bundles at rest in your S3 bucket, use SSE\-S3 or SSE\-KMS keys to allow AWS Proton to retrieve them\.