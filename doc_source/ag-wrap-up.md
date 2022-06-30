# Wrap up template files for AWS Proton<a name="ag-wrap-up"></a>

After preparing your environment and service infrastructure as code \(IaC\) files and their respective schema files, you must organize them in directories\. You must also create a manifest YAML file\. The manifest file lists the IaC files in a directory, the rendering engine, and the template language used to develop the IaC in this template\.

**Note**  
A manifest file can also be used independently of template bundles, as a direct input to *directly defined components*\. In this case, it always specifies a single IaC template file, for both CloudFormation and Terraform\. For more information about components, see [AWS Proton components](ag-components.md)\.

The manifest file needs to adhere to the format and content shown in the following example\.

**CloudFormation manifest file format:**

With CloudFormation, you list a single file\.

```
infrastructure:
  templates:
    - file: "cloudformation.yaml"
      rendering_engine: jinja
      template_language: cloudformation
```

**Terraform manifest file format:**

With terraform, you can explicitly list a single file or use the wildcard `*` to list each of the files in a directory\.

**Note**  
The wildcard only includes files whose names end with `.tf`\. Other files are ignored\.

```
infrastructure:
  templates:
    - file: "*"
      rendering_engine: hcl
      template_language: terraform
```

 After you set up the directories and manifest files for your environment or service template bundle, you gzip the directories into a tar ball upload them to an Amazon Simple Storage Service \(Amazon S3\) bucket where AWS Proton can retrieve them\.

When you create a minor version of an environment or a service template that you registered with AWS Proton, you provide the path to your environment or service template bundle tar ball that's located in your S3 bucket\. AWS Proton saves it with the new template minor version\. You can select the new template minor version to create or update environments or services with AWS Proton\.

## Environment template bundle wrap up<a name="environment-wrap-up"></a>

There are two types of environment template bundles that you create for AWS Proton\.
+ To create an environment template bundle for a *standard* environment template, organize the schema, infrastructure as code \(IaC\) files and manifest file in directories as shown in the following environment template bundle directory structure\.
+ To create an environment template bundle for a *customer managed* environment template, provide only the schema file and directory\. *Don't* include the infrastructure directory and files\. AWS Proton throws an error if the infrastructure directory and files are included\.

For more information, see [Register and publish templates](template-create.md)\.

CloudFormation environment template bundle directory structure:

```
 /schema
   schema.yaml
 /infrastructure
   manifest.yaml
   cloudformation.yaml
```

Terraform environment template bundle directory structure:

```
 /schema
   schema.yaml
 /infrastructure
   manifest.yaml
   environment.tf
```

## Service template bundle wrap up<a name="service-wrap-up"></a>

To create a service template bundle, you must organize the schema, infrastructure as code \(IaC\) files, and manifest files into directories as shown in the service template bundle directory structure example\.

If you *don’t* include a service pipeline in your template bundle, *don't* include the pipeline directory and files and set `"pipelineProvisioning": "CUSTOMER_MANAGED"` when you create the service template that is to be associated with this template bundle\.

**Note**  
You can't modify `pipelineProvisioning` after the service template is created\.

For more information, see [Register and publish templates](template-create.md)\.

CloudFormation service template bundle directory structure:

```
 /schema
   schema.yaml
 /instance_infrastructure
   manifest.yaml
   cloudformation.yaml
 /pipeline_infrastructure
   manifest.yaml
   cloudformation.yaml
```

Terraform service template bundle directory structure:

```
 /schema
   schema.yaml
 /instance_infrastructure
   manifest.yaml
   instance.tf
 /pipeline_infrastructure
   manifest.yaml
   pipeline.tf
```