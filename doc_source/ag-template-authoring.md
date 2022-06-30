# Authoring templates and creating bundles for AWS Proton<a name="ag-template-authoring"></a>

AWS Proton provisions resources for you based on infrastructure as code \(IaC\) files\. You describe infrastructure in reusable IaC files\. To make the files reusable for different environments and applications, you author them as *templates*, define input parameters, and use these parameters in IaC definitions\. When you later create a provisioning resource \(environment, service instance, or component\), AWS Proton uses a rendering engine, which combines input values with a template to create an IaC file that is ready to provision\.

Administrators author most templates as *template bundles*, and then upload and register them into AWS Proton\. The remainder of this page discusses these AWS Proton template bundles\. *Directly defined components* are an exception—developers create them and provide IaC template files directly\. For more information about components, see [AWS Proton components](ag-components.md)\.

**Topics**
+ [Template bundles](#ag-template-bundles)
+ [AWS Proton parameters](parameters.md)
+ [AWS Proton infrastructure as code files](ag-infrastructure-tmp-files.md)
+ [Schema file](ag-schema.md)
+ [Wrap up template files for AWS Proton](ag-wrap-up.md)
+ [Template bundle considerations](template-considerations.md)

## Template bundles<a name="ag-template-bundles"></a>

As an administrator, you [create and register templates](template-create.md) with AWS Proton\. You use these templates to create environments and services\. When you create a service, AWS Proton provisions and deploys service instances to selected environments\. For more information, see [AWS Proton for platform teams](Welcome.md#ag-admin)\.

To create and register a template in AWS Proton, you upload a template bundle that contains the infrastructure as code \(IaC\) files that AWS Proton needs to provision and environment or service\.

A *template bundle* contains the following:
+ An [Infrastructure as code \(IaC\) file](ag-infrastructure-tmp-files.md) with a [manifest YAML file](ag-wrap-up.md) that lists the *IaC file*\.
+ A [schema YAML file](ag-schema.md) for your IaC file input parameter definitions\.

A CloudFormation environment template bundle contains one IaC file\.

A CloudFormation service template bundle contains one IaC file for service instance definitions and another optional IaC file for a pipeline definition\.

Terraform environment and service template bundles can contain multiple IaC files each\.

AWS Proton requires an input parameter schema file\. When you use AWS CloudFormation to create your IaC files, you use [Jinja](https://jinja.palletsprojects.com/en/2.11.x/) syntax to reference your input parameters\. AWS Proton provides parameter namespaces that you can use to reference [parameters](parameters.md) in your IaC files\.

The following diagram shows an example of steps that you can take to create a *template* for AWS Proton\.

![\[A diagram that describes a process of how to create a template bundle for a set of AWS Proton infrastructure resources.\]](http://docs.aws.amazon.com/proton/latest/userguide/images/bundles.png)

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-one.png) Identify [input parameters](parameters.md)\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-two.png) Create a [schema file](ag-schema.md) to define your input parameters\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-three.png) Create [IaC files](ag-infrastructure-tmp-files.md) that reference your input parameters\. You can reference environment IaC file *outputs* as *inputs* for your service IaC files\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-four.png) [Register a template version](template-create.md) with AWS Proton and upload your template bundle\.