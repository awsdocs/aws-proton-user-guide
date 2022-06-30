# Versioned templates<a name="ag-template-versions"></a>

As an administrator or a member of a platform team, you define, create, and manage a library of versioned templates that are used to provision infrastructure resources\. There are two types of template versions—minor versions and major versions\.
+ *Minor versions* – Changes to the template that have a backward compatible schema\. These changes don't require the developer to provide new information when updating to the new template version\.

  When you attempt to make a minor version change, AWS Proton makes a best\-effort attempt to determine whether the schema of the new version is backward compatible with the previous minor versions of the template\. If the new schema isn't backward compatible, AWS Proton fails the registration of the new minor version\.
**Note**  
Compatibility is determined solely based on schema\. AWS Proton doesn't check if the template bundle infrastructure as code \(IaC\) file is backward compatible with the previous minor versions\. For example, AWS Proton doesn't check if the new IaC file causes breaking changes for the applications that are running on the infrastructure provisioned by a previous minor version of the template\.
+ *Major versions* – Changes to the template that may not be not backward compatible\. These changes typically require new inputs from the developer and often involve template schema changes\.

  You may sometimes choose to designate a backward compatible change as a major version based on your team’s operational model\.

The way AWS Proton determines if a template version request is for a minor or major version depends on the way template changes are tracked:
+ When you explicitly make a request to create a new template version, you request a major version by specifying a major version number, and you request a minor version by not specifying a major version number\.
+ When you use [template sync](ag-template-sync-configs.md) \(and therefore you don't make explicit template version requests\), AWS Proton attempts to create new minor versions for template changes that occur in the existing YAML file\. AWS Proton creates a major version when you create a new directory for the new template change \(for example, move from v1 to v2\)\.
**Note**  
A new minor version registration based on template sync still fails if AWS Proton determines that the change isn't backward compatible\.

When you publish a new version of a template, it becomes the **Recommended** version if it's the highest major and minor version\. New AWS Proton resources are created using the new recommended version, and AWS Proton prompts administrators to use the new version and to update existing AWS Proton resources that are using an outdated version\.