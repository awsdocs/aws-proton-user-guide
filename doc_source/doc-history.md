# Document history<a name="doc-history"></a>

The following table describes the important changes to the documentation related to the latest release of AWS Proton and customer feedback\. For notification about updates to this documentation, you can subscribe to an RSS feed\.
+ **API version: 2020\-07\-20**

| Change | Description | Date | 
| --- |--- |--- |
| [Service sync configurations\.](#doc-history) | AWS Proton adds support for [service sync configurations](https://docs.aws.amazon.com/proton/latest/userguide/ag-service-sync-configs.html)\. | March 31, 2023 | 
| [CodeBuild](#doc-history) | AWS Proton adds support for [CodeBuild provisioning](https://docs.aws.amazon.com/proton/latest/userguide/ag-infrastructure-tmp-files-codebuild.html)\. | November 16, 2022 | 
| [Managed policy update](#doc-history) | Added `[AWSProtonCodeBuildProvisioningBasicAccess](https://docs.aws.amazon.com/proton/latest/userguide/security-iam-awsmanpol.html#security-iam-awsmanpol-AWSProtonCodeBuildProvisioningBasicAccess)` policy that gives CodeBuild the permissions it needs to run a build for AWS Proton CodeBuild Provisioning\. | November 11, 2022 | 
| [Terraform tag propagation](#doc-history) | Added Terraform tag propagation to the [Tagging](resources.html) chapter\. | September 16, 2022 | 
| [API migration guide](#doc-history) | Removed the pre\-GA API migration guide\. | August 12, 2022 | 
| [AWS Proton objects](#doc-history) | Added a topic about AWS Proton objects and their relationship to other AWS and third\-party objects\. See [AWS Proton objects](ag-works-objects.html)\. | July 29, 2022 | 
| [Linked repository clarifications](#doc-history) | Clarified the purpose of linked \(registered\) repositories and their usage across the guide\. | July 18, 2022 | 
| [Guide merge](#doc-history) | Merged the two separate administrator and user guides into a single guide, the *AWS Proton User Guide*\. | June 30, 2022 | 
| [Managed policy update](#doc-history) | Updated managed policies to provide access to new AWS Proton API operations and to fix permission issues for some AWS Proton console operations\. See [AWS managed policies for AWS Proton](security-iam-awsmanpol.html)\. | June 20, 2022 | 
| [Getting started with the CLI](#doc-history) | Updated [Getting started with the AWS CLI](ag-getting-started-cli.html) with a new tutorial that uses the new template library\. | June 14, 2022 | 
| [Directly defined components](#doc-history) | Added the [Components](ag-components.html) chapter and made related modifications throughout the guide\. | June 1, 2022 | 
| [AWS Proton template library](#doc-history) | Added [The AWS Proton template library](ag-getting-started-templates.html) topic\. | May 6, 2022 | 
| [Terraform general availability \(GA\)](#doc-history) | Renamed *pull request provisioning* to *self\-managed provisioning*\. Added [Provisioning methods](ag-works-prov-methods.html) topic\. | March 23, 2022 | 
| [Repository tagging](#doc-history) | Added support for tagging Repository resources\. See [Create a link to your repository](ag-create-repo.html)\. | March 23, 2022 | 
| [Documentation update](#doc-history) | Added environment account connection tagging\. | November 26, 2021 | 
| [Template syncs and Terraform preview](#doc-history) | Added automated template versioning with the [template sync](ag-template-sync-configs.html) feature for general availability and [pull request provisioning with Terraform](ag-infrastructure-tmp-files.html) in preview\. API migration guide back in\. | November 24, 2021 | 
| [Documentation updates](#doc-history) | Added [EventBridge](event-tutorial-sns.html) tutorial, [Getting started workflow](ag-admin-workflow.html), [How AWS Proton works](ag-works.html), and [Template bundle](ag-template-authoring.html#ag-template-bundles) section enhancements\. | September 17, 2021 | 
| [AWS Proton console help panels release](#doc-history) | Help panels added to the console\. Console template version delete no longer deletes lower versions\. The API migration guide has been removed\. | September 8, 2021 | 
| [AWS Proton general availability \(GA\) release](#doc-history) | Added [cross account environments](ag-env-account-connections.html), [EventBridge monitoring](monitoring.html), [IAM condition keys](security_iam_service-with-iam.html), idempotency support, and [increased quotas](ag-limits.html)\. | June 9, 2021 | 
| [Add and delete service instances for a service and use existing external infrastructure for environments with AWS Proton](#doc-history) | This public preview release includes updates that make it possible for you to [add and delete service instances from a service](ag-svc-update.html), to [use your existing external infrastructure in an AWS Proton environment](template-create.html) and to cancel environment, service instance and pipeline deployments\. AWS Proton now supports [PrivateLink](infrastructure-security.html)\. An additional deletion validation has been added to prevent a minor version from being mistakenly deleted while a resource is using it\. | April 27, 2021 | 
| [Tagging with AWS Proton](#doc-history) | Public preview release 2 includes AWS Proton [tagging](resources.html) and the ability to launch services [without a service pipeline](ag-create-svc.html)\. | March 5, 2021 | 
| [Initial release](#doc-history) | Public preview release is now available in selected regions\. | December 1, 2020 | 