# CodeBuild provisioning parameter details and examples<a name="parameters-codebuild"></a>

You can define parameters in your templates for CodeBuild\-based AWS Proton resources and reference these parameters in your provisioning code\. For a detailed description of AWS Proton parameters, parameter types, the parameter namespace, and how to use parameters in your IaC files, see [AWS Proton parameters](parameters.md)\.

**Note**  
You can use CodeBuild provisioning with environments and services\. At this time you can't provision components this way\.

## Input parameters<a name="parameters-codebuild.input"></a>

When you create an AWS Proton resource, like an environment or a service, you provide values for input parameters that are defined in your template's [schema file](ag-schema.md)\. When the resource that you create uses [CodeBuild provisioning](ag-works-prov-methods.md#ag-works-prov-methods-codebuild), AWS Proton renders these input values into an input file\. Your provisioning code can import and get parameter values from this file\.

You can configure the format of the generated input file by using the `input_type` property of the template's manifest file\. This property's values can be `JSON` \(the default\) or `HCL`\. For an example of a CodeBuild templates, see [CodeBuild provisioning template bundle](ag-infrastructure-tmp-files-codebuild.md)\. For more information about manifest files, see [Wrap up template files for AWS Proton](ag-wrap-up.md)\.

The following example is a JSON input file generated during CodeBuild\-based provisioning of a service instance\.

### Example: using the AWS CDK with CodeBuild provisioning<a name="parameters-codebuild.example"></a>

```
{
  "service_instance": {
    "name": "my-service-staging",
    "inputs": {
      "port": "8080",
      "task_size": "medium"
    }
  },
  "service": {
    "name": "my-service"
  },
  "environment": {
    "account_id": "123456789012",
    "name": "my-env-staging",
    "outputs": {
      "vpc-id": "hdh2323423"
    }
  }
}
```

## Output parameters<a name="parameters-codebuild.output"></a>

To communicate resource provisioning outputs back to AWS Proton, your provisioning code can generate a JSON file named `proton-outputs.json` with values for output parameters defined in your template's [schema file](ag-schema.md)\. For example, the `cdk deploy` command has the `--outputs-file` argument that instructs the AWS CDK to generate a JSON file with provisioning outputs\. If your resource uses the AWS CDK, specify the following command in your CodeBuild template manifest:

```
aws proton notify-resource-deployment-status-change
```

AWS Proton looks for this JSON file\. If the file exists after your provisioning code successfully completes, AWS Proton reads output parameter values from it\.