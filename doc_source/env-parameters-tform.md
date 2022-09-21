# Terraform infrastructure as code \(IaC\) file parameter details and examples<a name="env-parameters-tform"></a>

You can include Terraform input variables in `variable.tf` files in your template bundle\. You can also create a schema to create AWS Proton managed variables\. AWS Proton creates variable `.tf files` from your schema file\. For more information, see [Terraform IaC files](ag-infrastructure-tmp-files.md#terraform)\.

To reference your schema defined AWS Proton variables in your infrastructure `.tf files`, you use the AWS Proton namespaces shown in the *Parameters and namespaces for Terraform IaC* table\. For example, you can use `var.environment.inputs.vpc_cidr`\. Inside quotation marks, surround these variables with single brackets and add a dollar sign in front of the first brace \(for example, `“${var.environment.inputs.vpc_cidr}”`\)\.

The following example shows how to use namespaces to include AWS Proton parameters in an environment `.tf file`\.

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