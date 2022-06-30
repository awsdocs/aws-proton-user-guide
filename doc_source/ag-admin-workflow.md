# Getting started workflow<a name="ag-admin-workflow"></a>

Learn to create template bundles, create and register templates, and create environments and services by following the example steps and links\.

Before starting, verify that you created an [AWS Proton service role](ag-setting-up-iam.md#setting-up-cicd)\.

If your service template includes an AWS Proton service pipeline, verify that you created an [AWS CodeStar connection](setting-up-for-service.md#setting-up-vcontrol) and a [AWS Proton pipeline service role](ag-setting-up-iam.md#setting-up-cicd)\.

For more information, see [The AWS Proton service API Reference](https://docs.aws.amazon.com/proton/latest/APIReference/Welcome.html)\.

**Example: Getting started workflow**

1. Refer to the diagram in [How AWS Proton works](ag-works.md) for a high\-level view of AWS Proton inputs and outputs\.

1. [Create an environment bundle and a service template bundle](ag-template-authoring.md#ag-template-bundles)\.

   1. Identify [input parameters](parameters.md)\.

   1. Create a [schema file](ag-schema.md)\.

   1. Create [infrastructure as code \(IaC\) files](ag-infrastructure-tmp-files.md)\.

   1. To [wrap up your template bundle](ag-wrap-up.md), create a manifest file and organize your IaC files, manifest files, and schema file in directories\.

   1. Make your [template bundle](ag-wrap-up.md) accessible to AWS Proton\.

1. [Create and register an environment template version](template-create.md) with AWS Proton\.

   When you use the console to create and register a template, a template version is automatically created\.

   When you use the AWS CLI to create and register a template:

   1. Create an environment template\.

   1. Create an environment template version\.

   For more information, see [CreateEnvironmentTemplate](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateEnvironmentTemplate.html) and [CreateEnvironmentTemplateVersion](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateEnvironmentTemplateVersion.html) in the *AWS Proton API reference*\.

1. [Publish your environment template](template-update.md) to make it available for use\.

   For more information, see [UpdateEnvironmentTemplateVersion](https://docs.aws.amazon.com/proton/latest/APIReference/API_UpdateEnvironmentTemplateVersion.html) in the *AWS Proton API reference*\.

1. To [create an environment](ag-create-env.md), select a published environment template version and provide values for required inputs\.

   For more information, see [CreateEnvironment](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateEnvironment.html) in the *AWS Proton API reference*\.

1. [Create and register a service template version](template-create.md) with AWS Proton\.

   When you use the console to create and register a template, a template version is automatically created\.

   When you use the AWS CLI to create and register a template:

   1. Create a service template\.

   1. Create a service template version\.

   For more information, see [CreateServiceTemplate](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateServiceTemplate.html) and [CreateServiceTemplateVersion](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateServiceTemplateVersion.html) in the *AWS Proton API reference*\.

1. [Publish your service template](template-update.md) to make it available for use\.

   For more information, see [UpdateServiceTemplateVersion](https://docs.aws.amazon.com/proton/latest/APIReference/API_UpdateServiceTemplateVersion.html) in the *AWS Proton API reference*\.

1. To [create a service](ag-create-svc.md), select a published service template version and provide values for required inputs\.

   For more information, see [CreateService](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateService.html) in the *AWS Proton API reference*\.