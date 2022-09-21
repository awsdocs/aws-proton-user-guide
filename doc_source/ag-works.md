# How AWS Proton works<a name="ag-works"></a>

With AWS Proton, you provision *environments*, and then *services* running in those environments\. Environments and services are based on environment and service *templates*, respectively, that you choose in your AWS Proton versioned template library\.

![\[A diagram that describes a process of how to create a template bundle for a set of AWS Proton infrastructure resources.\]](http://docs.aws.amazon.com/proton/latest/userguide/images/works.png)

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-one.png) When you, as an administrator, select an environment template with AWS Proton, you provide values for required *input parameters*\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-two.png) AWS Proton uses the environment template and parameter values to provision your environment\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-three.png) When you, as a developer or administrator, select a service template with AWS Proton, you provide values for required input parameters\. You also select an environment to deploy your application or service to\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-four.png) AWS Proton uses the service template, and both your service and selected environment parameter values, to provision your service\.

You provide values for the input parameters to customize your template for re\-use and multiple use cases, applications, or services\.

To make this work, you create environment or service template bundles and upload them to registered environment or service templates, respectively\.

[Template bundles](ag-template-authoring.md#ag-template-bundles) contain everything AWS Proton needs to provision environments or services\.

When you create an environment or service template, you upload a template bundle that contains the parametrized infrastructure as code \(IaC\) files that AWS Proton uses to provision environments or services\.

When you select an environment or service template to create or update an environment or service, you provide values for the template bundle IaC file parameters\.

**Topics**
+ [AWS Proton objects](ag-works-objects.md)
+ [How AWS Proton provisions infrastructure](ag-works-prov-methods.md)
+ [AWS Proton terminology](terminology.md)