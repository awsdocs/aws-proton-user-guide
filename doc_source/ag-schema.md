# Schema file<a name="ag-schema"></a>

As an administrator, when you use the Open API [Data Models \(schemas\) section](https://swagger.io/docs/specification/data-models/) to define a parameter schema YAML file for your template bundle, AWS Proton can validate parameter value inputs against the requirements that you defined in your schema\.

For more information about formats and available keywords, see the [Schema object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.3.md#schemaObject) section of the OpenAPI\.

## Schema requirements for environment template bundles<a name="schema-req-env"></a>

Your schema must follow the [Data Models \(schemas\) section](https://swagger.io/docs/specification/data-models/) of the OpenAPI in the YAML format\. It must also be a part of your environment template bundle\.

For your environment schema, you must include the formatted headers to establish that you're using the Data Models \(schemas\) section of the Open API\. In the following environment schema examples, these headers appear in the first three lines\.

An `environment_input_type` must be included and defined with a name that you provide\. In the following examples, this is defined on line 5\. By defining this parameter, you associate it with an AWS Proton environment resource\. 

To follow the Open API schema model, you must include `types`\. In the following example, this is line 6\.

Following `types`, you must define an `environment_input_type` type\. You define the input parameters for your environment as properties of the `environment_input_type`\. You must include at least one property with a name that matches at least one parameter that's listed in the environment infrastructure as code \(IaC\) file that's associated with schema\.

When you create an environment and provide customized parameter values, AWS Proton uses the schema file to match, validate, and inject them into the curly braced parameters in the associated CloudFormation IaC file\. For each property \(parameter\), provide a `name` and `type`\. Optionally, also provide a `description`, `default`,and `pattern`\.

The defined parameters for the following example *standard* environment template schema include `vpc_cidr`, `subnet_one_cidr`, and `subnet_two_cidr` with the `default` keyword and default values\. When you create an environment with this environment template bundle schema, you can accept the default values or provide your own\. If a parameter *doesn't* have a default value and is listed as a `required` property \(parameter\), you must provide values for it when you create an environment\.

The second example *standard* environment template schema lists the `required` parameter `my_other_sample_input`\.

You can create a schema for two types of environment templates\. For more information, see [Register and publish templates](template-create.md)\.
+ ***Standard* environment templates**

  In the following example, an environment input type is defined with a description and input properties\. This schema example can be used with the AWS Proton CloudFormation IaC file shown in [Example 3](ag-infrastructure-tmp-files.md#ag-proton-env-cfn-example)\.

  Example schema for a *standard* environment template:

  ```
  schema:                            # required
    format:                          # required
      openapi: "3.0.0"               # required
    # required              defined by administrator
    environment_input_type: "PublicEnvironmentInput"
    types:                           # required
      # defined by administrator
      PublicEnvironmentInput:
        type: object
        description: "Input properties for my environment"
        properties:
          vpc_cidr:                   # parameter
            type: string
            description: "This CIDR range for your VPC"
            default: 10.0.0.0/16
            pattern: ([0-9]{1,3}\.){3}[0-9]{1,3}($|/(16|24))
          subnet_one_cidr:            # parameter
            type: string
            description: "The CIDR range for subnet one"
            default: 10.0.0.0/24
            pattern: ([0-9]{1,3}\.){3}[0-9]{1,3}($|/(16|24))
          subnet_two_cidr:            # parameter
            type: string
            description: "The CIDR range for subnet one"
            default: 10.0.1.0/24
            pattern: ([0-9]{1,3}\.){3}[0-9]{1,3}($|/(16|24))
  ```

  Example schema for a *standard* environment template that includes a `required` parameter:

  ```
  schema:                            # required
    format:                          # required
      openapi: "3.0.0"               # required
    # required              defined by administrator
    environment_input_type: "MyEnvironmentInputType"
    types:                           # required
      # defined by administrator
      MyEnvironmentInputType:
        type: object
        description: "Input properties for my environment"
        properties:
          my_sample_input:           # parameter
            type: string
            description: "This is a sample input"
            default: "hello world"
          my_other_sample_input:     # parameter
            type: string
            description: "Another sample input"
          another_optional_input:    # parameter
            type: string
            description: "Another optional input"
            default: "!"
        required:
          - my_other_sample_input
  ```
+ ***Customer managed* environment templates**

  In the following example, the schema only includes a list of outputs that replicate the outputs from the IaC that you used to provision your *customer managed* infrastructure\. You need to define output value types as *strings only* \(*not* lists, arrays or other types\)\. For example, the next code snippet shows the outputs section of an external AWS CloudFormation template\. This is from the template shown in [Example 1](ag-infrastructure-tmp-files.md#ag-env-cfn-example)\. It can be used to create external *customer managed* infrastructure for an AWS Proton Fargate service created from [Example 4](ag-infrastructure-tmp-files.md#ag-proton-svc-cfn-example)\.
**Important**  
As an administrator, you must ensure that your provisioned and managed infrastructure and all output parameters are compatible with the associated *customer managed* environment templates\. AWS Proton can't account for changes on your behalf because these changes aren't visible to AWS Proton\. Inconsistencies result in failures\.

  Example CloudFormation IaC file outputs for a *customer managed* environment template:

  ```
  // Cloudformation Template Outputs
  [...]
  Outputs:
    ClusterName:
      Description: The name of the ECS cluster
      Value: !Ref 'ECSCluster'
    ECSTaskExecutionRole:
      Description: The ARN of the ECS role
      Value: !GetAtt 'ECSTaskExecutionRole.Arn'
    VpcId:
      Description: The ID of the VPC that this stack is deployed in
      Value: !Ref 'VPC'
  [...]
  ```

  The schema for the corresponding AWS Proton *customer managed* environment template bundle is shown in the following example\. Each output value is defined as a string\.

  Example schema for a *customer managed* environment template:

  ```
  schema:                            # required
    format:                          # required
      openapi: "3.0.0"               # required
    # required              defined by administrator
    environment_input_type: "EnvironmentOutput"
    types:                           # required
      # defined by administrator
      EnvironmentOutput:
        type: object
        description: "Outputs of the environment"
        properties:
          ClusterName:               # parameter
            type: string
            description: "The name of the ECS cluster"
          ECSTaskExecutionRole:      # parameter
            type: string
            description: "The ARN of the ECS role"
          VpcId:                     # parameter
            type: string
            description: "The ID of the VPC that this stack is deployed in"
  [...]
  ```

## Schema requirements for service template bundles<a name="schema-req-svc"></a>

Your schema must follow the [Data Models \(schemas\) section](https://swagger.io/docs/specification/data-models/) of the OpenAPI in YAML format as shown in the following examples\. You must provide a schema file in your service template bundle\.

In the following service schema examples, you must include the formatted headers\. In the following example, this is in the first three lines\. This is to establish that you're using the Data Models \(schemas\) section of the Open API\.

A `service_input_type` must be included and defined with a name that you provide\. In the following example, this is in line 5\. This associates the parameters with an AWS Proton service resource\.

An AWS Proton service pipeline is included by default when you use the console or the CLI to create a service\. When you include a service pipeline for your service, you must include `pipeline_input_type` with a name that you provide\. In the following example, this is in line 7\. *Don’t* include this parameter if you *aren’t* including an AWS Proton service pipeline\. For more information, see [Register and publish templates](template-create.md)\.

To follow the Open API schema model, you must include `types` In the following example, this is in line 9\.

Following `types`, you must define a `service_input_type` type\. You define the input parameters for your service as properties of the `service_input_type`\. You must include at least one property with a name that matches at least one parameter listed in the service infrastructure as code \(IaC\) file that is associated with schema\.

To define a service pipeline, below your `service_input_type` definition, you must define a `pipeline_input_type`\. As above, you must include at least one property with a name that matches at least one parameter listed in a pipeline IaC file that is associated with schema\. *Don’t* include this definition if you *aren’t* including an AWS Proton service pipeline\.

When you, as an administrator or developer, create a service and provide customized parameter values, AWS Proton uses the schema file to match, validate, and inject them into the associated CloudFormation IaC file’s curly braced parameters\. For each property \(parameter\), provide a `name` and a `type`\. Optionally, also provide a `description`, `default`, and `pattern`\.

The defined parameters for the example schema include `port`, `desired_count`, `task_size` and `image` with the `default` keyword and default values\. When you create a service with this service template bundle schema, you can accept the default values or provide your own\. The parameter `unique_name` is also included in the example and *doesn't* have a default value\. It is listed as a `required` property \(parameter\)\. You, as administrator or developer, must provide values for `required` parameters when you create services\.

If you want to create a service template with a service pipeline, include the `pipeline_input_type` in your schema\.

**Example service schema file for a service that includes an AWS Proton service pipeline\.**

 This schema example can be used with the AWS Proton IaC files shown in [Example 4](ag-infrastructure-tmp-files.md#ag-proton-svc-cfn-example) and [Example 5](ag-infrastructure-tmp-files.md#ag-proton-pipeline-cfn-example)\. A service pipeline is included\.

```
schema:                            # required
  format:                          # required
    openapi: "3.0.0"               # required
  # required           defined by administrator
  service_input_type: "LoadBalancedServiceInput"
  # only include if including AWS Proton service pipeline, defined by administrator
  pipeline_input_type: "PipelineInputs"

  types:                           # required
    # defined by administrator
    LoadBalancedServiceInput:
      type: object
      description: "Input properties for a loadbalanced Fargate service"
      properties:
        port:                      # parameter
          type: number
          description: "The port to route traffic to"
          default: 80
          minimum: 0
          maximum: 65535
        desired_count:             # parameter
          type: number
          description: "The default number of Fargate tasks you want running"
          default: 1
          minimum: 1
        task_size:                 # parameter
          type: string
          description: "The size of the task you want to run"
          enum: ["x-small", "small", "medium", "large", "x-large"]
          default: "x-small"
        image:                     # parameter
          type: string
          description: "The name/url of the container image"
          default: "public.ecr.aws/z9d2n7e1/nginx:1.19.5"
          minLength: 1
          maxLength: 200
        unique_name:               # parameter
          type: string
          description: "The unique name of your service identifier. This will be used to name your log group, task definition and ECS service"
          minLength: 1
          maxLength: 100
      required:
        - unique_name
    # defined by administrator
    PipelineInputs:
      type: object
      description: "Pipeline input properties"
      properties:
        dockerfile:                # parameter
          type: string
          description: "The location of the Dockerfile to build"
          default: "Dockerfile"
          minLength: 1
          maxLength: 100
        unit_test_command:         # parameter
          type: string
          description: "The command to run to unit test the application code"
          default: "echo 'add your unit test command here'"
          minLength: 1
          maxLength: 200
```

If you want to create a service template without a service pipeline, *don't* include the `pipeline_input_type` in your schema, as shown in the following example\.

**Example service schema file for a service that *doesn't* include an AWS Proton service pipeline**

```
schema:                            # required
  format:                          # required
    openapi: "3.0.0"               # required
  # required            defined by administrator  
  service_input_type: "MyServiceInstanceInputType"

  types:                           # required
    # defined by administrator
    MyServiceInstanceInputType:
      type: object
      description: "Service instance input properties"
      required:
        - my_sample_service_instance_required_input
      properties:
        my_sample_service_instance_optional_input:   # parameter
          type: string
          description: "This is a sample input"
          default: "hello world"
        my_sample_service_instance_required_input:   # parameter
          type: string
          description: "Another sample input"
```