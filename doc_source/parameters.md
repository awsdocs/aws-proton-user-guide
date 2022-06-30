# AWS Proton parameters<a name="parameters"></a>

You can define and use parameters in your infrastructure as code \(IaC\) files to make them flexible and re\-usable\. You read a parameter value in your IaC files by referring to the parameter's name in the AWS Proton *parameter namespace*\. AWS Proton injects parameter values into the rendered IaC files that it generates during resource provisioning\. To process AWS CloudFormation IaC parameters, AWS Proton uses [Jinja](https://jinja.palletsprojects.com/en/2.11.x/)\. To process Terraform IaC parameters, AWS Proton generates a Terraform parameter value file and relies on the parametrization ability built into HCL\.

You can refer to parameters in your environment, service, and component IaC files with the following requirements:
+ The length of each parameter name doesn't exceed 100 characters\.
+ The length of the parameter namespace and resource name combined doesn't exceed the character limit for the resource name\.

AWS Proton provisioning fails if these quotas are exceeded\.

## Parameter types<a name="param-name-types"></a>

The following parameter types are available to you for reference in AWS Proton IaC files:

**Input parameter**  
Environments and service instances can take input parameters that you define in a [schema file](ag-schema.md) that you associate with the environment or service template\. You can refer to a resource's input parameters in the resource's IaC file\. Component IaC files can refer to input parameters of the service instance that the component is attached to\.  
AWS Proton checks input parameter names against your schema file, and matches them with the parameters that are referenced in your IaC files to inject the input values that you provide in a spec file during resource provisioning\.

**Output parameter**  
You can define outputs in any of your IaC files\. An output can be, for example, a name, ID, or ARN of one of the resources that the template provisions, or it can be a way to pass through one of the template's inputs\. You can refer to these outputs in IaC files of other resources\.  
In CloudFormation IaC files,define output parameters in the `Outputs:` block\. In a Terraform IaC file, define each output parameter using an `output` statement\.

**Resource parameter**  
AWS Proton automatically creates AWS Proton resource parameters\. These parameters expose properties of the AWS Proton resource object\. An example of a resource parameter is `environment.name`\.

## Using AWS Proton parameters in your IaC files<a name="param-name-spaces"></a>

To read a parameter value in an IaC file, you refer to the parameter's name in the AWS Proton parameter namespace\. For AWS CloudFormation IaC files, you use *Jinja* syntax and surround the parameter with pairs of curly braces and quotation marks\.

The following table shows the reference syntax for each supported template language, with an example\.


| Template language | Syntax | Example: environment input named "VPC" | 
| --- | --- | --- | 
|  CloudFormation  |  `"{{ parameter-name }}"`  |  `"{{ environment.inputs.VPC }}"`  | 
|  Terraform  |  `var.parameter-name`  |  `var.environment.inputs.VPC` [Generated Terraform variable definitions](ag-infrastructure-tmp-files.md#compiled-tform)  | 

**Note**  
If you use [CloudFormation dynamic parameters](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/dynamic-references.html) in your IaC file, you must [escape them](https://jinja.palletsprojects.com/en/2.11.x/templates/#escaping) to prevent Jinja misinterpretation errors\. For more information, see [Troubleshooting AWS Proton](ag-troubleshooting.md)

The following table lists namespace names for all AWS Proton resource parameters\. Each template file type can use a different subset of the parameter namespace\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/proton/latest/userguide/parameters.html)

For more information and examples, see the subtopics about parameters in IaC template files for different resource types and template languages\.

**Topics**
+ [Parameter types](#param-name-types)
+ [Using AWS Proton parameters in your IaC files](#param-name-spaces)
+ [Environment CloudFormation IaC file parameter details and examples](env-parameters.md)
+ [Service CloudFormation IaC file parameter details and examples](svc-parameters.md)
+ [Component CloudFormation IaC file parameter details and examples](comp-parameters.md)
+ [Parameter filters for CloudFormation IaC files](parameter-filters.md)
+ [Terraform infrastructure as code \(IaC\) file parameter details and examples](env-parameters-tform.md)