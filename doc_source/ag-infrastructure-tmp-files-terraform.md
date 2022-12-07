# Terraform IaC files<a name="ag-infrastructure-tmp-files-terraform"></a>

Learn how to use Terraform infrastructure as code \(IaC\) files with AWS Proton\. [Terraform](https://www.terraform.io/) is a widely used open\-source IaC engine that was developed by [HashiCorp](https://www.hashicorp.com/)\. Terraform modules are developed in HashiCorp's HCL language, and support several backend infrastructures providers, including Amazon Web Services\.

AWS Proton supports [self\-managed provisioning](ag-works-prov-methods.md#ag-works-prov-methods-self) for Terraform IaC\.

For a complete example of a provisioning repository that responds to pull requests and implements infrastructure provisioning, see [Terraform OpenSource GitHub Actions automation template for AWS Proton](https://github.com/aws-samples/aws-proton-terraform-github-actions-sample) on GitHub\.

**How self\-managed provisioning works with Terraform IaC template bundle files:**

1. When you [create an environment](ag-create-env.md) from Terraform template bundles, AWS Proton compiles your `.tf` files with console or `spec file` input parameters\.

1. It makes a pull request to merge the compiled IaC files to [repository that you have registered with AWS Proton](ag-create-repo.md)\.

1. If the request is approved, AWS Proton waits on provisioning status that you provide\.

1. If the request is rejected, the environment creation is cancelled\.

1. If the pull request times out, environment creation *isn't* complete\.

**AWS Proton with Terraform IaC considerations:**
+ AWS Proton doesn’t manage your Terraform provisioning\.
+ You must [register a provisioning repository](ag-create-repo.md) with AWS Proton\. AWS Proton makes pull requests on this repository\.
+ You must [create a CodeStar connection](setting-up-for-service.md#setting-up-vcontrol) to connect AWS Proton with your provisioning repository\.
+ To provision from AWS Proton compiled IaC files, you must respond to AWS Proton pull requests\. AWS Proton makes pull requests after environment and service create and update actions\. For more information, see [AWS Proton environments](ag-environments.md) and [AWS Proton services](ag-services.md)\.
+ To provision a pipeline from AWS Proton compiled IaC files, you must [create a CI/CD pipeline repository](setting-up-for-service.md#setting-up-pr-repo)\.
+ Your pull request based provisioning automation must include steps to notify AWS Proton of any provisioned AWS Proton resource status changes\. You can use the AWS Proton [NotifyResourceDeploymentStatusChange API](https://docs.aws.amazon.com/proton/latest/APIReference/API_NotifyResourceDeploymentStatusChange.html)\.
+ You can’t deploy services, pipelines, and components created from CloudFormation IaC files to environments created from Terraform IaC files\.
+ You can’t deploy services, pipelines, and components created from Terraform IaC files to environments created from CloudFormation IaC files\.

When preparing your Terraform IaC files for AWS Proton, you attach namespaces to your input variables, as shown in the following examples\. For more information, see [Parameters](parameters.md)\.

## Example 1: AWS Proton environment Terraform IaC file<a name="ag-env-tform-example"></a>

```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.0"
    }
  }
  // This tells terraform to store the state file in s3 at the location
  // s3://terraform-state-bucket/tf-os-sample/terraform.tfstate
  backend "s3" {
    bucket = "terraform-state-bucket"
    key    = "tf-os-sample/terraform.tfstate"
    region = "us-east-1"
  }
}

// Configure the AWS Provider
provider "aws" {
  region = "us-east-1"
  default_tags {
    tags = var.proton_tags
  }
}

resource "aws_ssm_parameter" "my_ssm_parameter" {
  name  = "my_ssm_parameter"
  type  = "String"
  // Use the Proton environment.inputs. namespace
  value = var.environment.inputs.ssm_parameter_value
}
```

## Compiled infrastructure as code<a name="compiled-tform"></a>

When you create an environment or service, AWS Proton compiles your infrastructure as code files with console or `spec file` inputs\. It creates `proton.resource-type.variables.tf` and `proton.auto.tfvars.json` files for your inputs that can be used by Terraform, as shown in the following examples\. These files are located in a specified repository in a folder that matches the environment or service instance name\.

The example shows how AWS Proton includes tags in the variable definition and variable values, and how you can propagate these AWS Proton tags to provisioned resources\. For more information, see [Tag propagation to provisioned resources](resources.md#auto-tags-prop)\.

### Example 2: compiled IaC files for an environment named "dev"\.<a name="ag-compiled-example"></a>

**dev/environment\.tf:**

```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.0"
    }
  }
  // This tells terraform to store the state file in s3 at the location
  // s3://terraform-state-bucket/tf-os-sample/terraform.tfstate
  backend "s3" {
    bucket = "terraform-state-bucket"
    key    = "tf-os-sample/terraform.tfstate"
    region = "us-east-1"
  }
}

// Configure the AWS Provider
provider "aws" {
  region = "us-east-1"
  default_tags {
    tags = var.proton_tags
  }
}

resource "aws_ssm_parameter" "my_ssm_parameter" {
  name  = "my_ssm_parameter"
  type  = "String"
  // Use the Proton environment.inputs. namespace
  value = var.environment.inputs.ssm_parameter_value
}
```

**dev/proton\.environment\.variables\.tf:**

```
variable "environment" {
  type = object({
    inputs = map(string)
    name = string
  })
}

variable "proton_tags" {
  type = map(string)
  default = null
}
```

**dev/proton\.auto\.tfvars\.json:**

```
{
  "environment": {
    "name": "dev",
    "inputs": {
      "ssm_parameter_value": "MyNewParamValue"
    }
  }

  "proton_tags" : {
    "proton:account" : "123456789012",
    "proton:template" : "arn:aws:proton:us-east-1:123456789012:environment-template/fargate-env",
    "proton:environment" : "arn:aws:proton:us-east-1:123456789012:environment/dev"
  }
}
```

## Repository paths<a name="repo-dir"></a>

AWS Proton uses console or spec inputs from environment or service create actions to find the repository and path where it is to locate the compiled IaC files\. The input values are passed to [namespaced input parameters](parameters.md)\.

AWS Proton supports two repository path layouts\. In the following examples, the paths are named by the namespaced resource parameters from two environments\. Each environment has service instances of two services, and the service instances of one of the services have directly defined components\.

<a name="limits-table"></a>[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/proton/latest/userguide/ag-infrastructure-tmp-files-terraform.html)

------
#### [ Layout 1 ]

If AWS Proton finds the specified repository with an `environments` folder, it creates a folder that includes the compiled IaC files and is named with the `environment.name`\.

If AWS Proton finds the specified repository with an `environments` folder that contains a folder name that matches a service instance compatible environment name, it creates a folder that includes the compiled instance IaC files and is named with the `service_instance.name`\.

```
/repo
    /environments
        /env-prod                             # environment folder
            main.tf
            proton.environment.variables.tf
            proton.auto.tfvars.json
          
            /service-one-instance-one-prod    # instance folder
                main.tf
                proton.service_instance.variables.tf
                proton.auto.tfvars.json
              
            /service-two-instance-two-prod    # instance folder
                main.tf
                proton.service_instance.variables.tf
                proton.auto.tfvars.json
              
            /component-prod                   # component folder
                main.tf
                proton.component.variables.tf
                proton.auto.tfvars.json
              
        /env-staged                           # environment folder
            main.tf
            proton.variables.tf
            proton.auto.tfvars.json         
          
            /service-one-instance-one-staged  # instance folder
                main.tf
                proton.service_instance.variables.tf
                proton.auto.tfvars.json
              
            /service-two-instance-two-staged  # instance folder
                main.tf
                proton.service_instance.variables.tf
                proton.auto.tfvars.json
              
            /component-staged                 # component folder
                main.tf
                proton.component.variables.tf
                proton.auto.tfvars.json
```

------
#### [ Layout 2 ]

If AWS Proton finds the specified repository without an `environments` folder, it creates an `environment.name` folder where it locates the compiled environment IaC files\.

If AWS Proton finds the specified repository with a folder name that matches a service instance compatible environment name, it creates a `service_instance.name` folder where it locates the compiled instance IaC files\.

```
/repo
    /env-prod                             # environment folder
        main.tf
        proton.environment.variables.tf
        proton.auto.tfvars.json
      
        /service-one-instance-one-prod    # instance folder
            main.tf
            proton.service_instance.variables.tf
            proton.auto.tfvars.json
          
        /service-two-instance-two-prod    # instance folder
            main.tf
            proton.service_instance.variables.tf
            proton.auto.tfvars.json
          
        /component-prod                   # component folder
            main.tf
            proton.component.variables.tf
            proton.auto.tfvars.json
          
    /env-staged                           # environment folder
        main.tf
        proton.variables.tf
        proton.auto.tfvars.json         
      
        /service-one-instance-one-staged  # instance folder
            main.tf
            proton.service_instance.variables.tf
            proton.auto.tfvars.json
          
        /service-two-instance-two-staged  # instance folder
            main.tf
            proton.service_instance.variables.tf
            proton.auto.tfvars.json
          
        /component-staged                 # component folder
            main.tf
            proton.component.variables.tf
            proton.auto.tfvars.json
```

------