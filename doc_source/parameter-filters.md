# Parameter filters for CloudFormation IaC files<a name="parameter-filters"></a>

When you make references to [AWS Proton parameters](parameters.md) in your AWS CloudFormation IaC files, you can use Jinja modifiers known as *filters* to validate, filter, and format parameter values before they get inserted into the rendered template\. Filter validations are particularly useful when referring to [component](ag-components.md) output parameters, because component creation and attachment are done by developers, and an administrator using component outputs in a service instance template might want to verify their existence and validity\. However, you can use filters in any Jinja IaC file\.

The following sections describe and define the available parameter filters, and provide examples\. AWS Proton defines most of these filters\. The `default` filter is a Jinja built\-in filter\.

## Format environment properties for Amazon ECS tasks<a name="parameter-filters.proton.cfn-ecs"></a>

**Declaration**

```
dict → proton_cfn_ecs_task_definition_formatted_env_vars (raw: boolean = True) → YAML list of dicts
```

**Description**

This filter formats a list of outputs to be used in an [Environment property](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ecs-taskdefinition-containerdefinitions.html#cfn-ecs-taskdefinition-containerdefinition-environment) in the `ContainerDefinition` section of an Amazon Elastic Container Service \(Amazon ECS\) task definition\.

Set `raw` to `False` to also validate the parameter value\. In this case, the value is required to match the regular expression `^[a-zA-Z0-9_-]*$`\. If the value fails this validation, template rendering fails\.

### Example<a name="parameter-filters.proton.cfn-ecs.example"></a>

With the following custom component template:

```
Resources:
  # ...
Outputs:
  Output1:
    Description: "Example component output 1"
    Value: hello
  Output2:
    Description: "Example component output 2"
    Value: world
```

And the following service template:

```
Resources:
  TaskDefinition:
    Type: AWS::ECS::TaskDefinition
    Properties:
      # ...
      ContainerDefinitions:
        - Name: MyServiceName
          # ...
          Environment:
            {{ service_instance.components.default.outputs
              | proton_cfn_ecs_task_definition_formatted_env_vars }}
```

The rendered service template is as follows:

```
Resources:
  TaskDefinition:
    Type: AWS::ECS::TaskDefinition
    Properties:
      # ...
      ContainerDefinitions:
        - Name: MyServiceName
          # ...
          Environment:
            - Name: Output1
              Value: hello
            - Name: Output2
              Value: world
```

## Format environment properties for Lambda functions<a name="parameter-filters.proton.cfn-lambda"></a>

**Declaration**

```
dict → proton_cfn_lambda_function_formatted_env_vars (raw: boolean = True) → YAML dict
```

**Description**

This filter formats a list of outputs to be used in an [Environment property](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-lambda-function.html#cfn-lambda-function-environment) in the `Properties` section of an AWS Lambda function definition\.

Set `raw` to `False` to also validate the parameter value\. In this case, the value is required to match the regular expression `^[a-zA-Z0-9_-]*$`\. If the value fails this validation, template rendering fails\.

### Example<a name="parameter-filters.proton.cfn-lambda.example"></a>

With the following custom component template:

```
Resources:
  # ...
Outputs:
  Output1:
    Description: "Example component output 1"
    Value: hello
  Output2:
    Description: "Example component output 2"
    Value: world
```

And the following service template:

```
Resources:
  Lambda:
    Type: AWS::Lambda::Function
    Properties:
      Environment:
        Variables:
          {{ service_instance.components.default.outputs
            | proton_cfn_lambda_function_formatted_env_vars }}
```

The rendered service template is as follows:

```
Resources:
  Lambda:
    Type: AWS::Lambda::Function
    Properties:
      Environment:
        Variables:
          Output1: hello
          Output2: world
```

## Extract IAM policy ARNs to include in IAM roles<a name="parameter-filters.proton.cfn-policy-arns"></a>

**Declaration**

```
dict → proton_cfn_iam_policy_arns → YAML list
```

**Description**

This filter formats a list of outputs to be used in a [ManagedPolicyArns property](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-role.html#cfn-iam-role-managepolicyarns) in the `Properties` section of an AWS Identity and Access Management \(IAM\) role definition\. The filter uses the regular expression `^arn:[a-zA-Z-]+:iam::\d{12}:policy/` to extract valid IAM policy ARNs from the list of output parameters\. You can use this filter to append policies in output parameter values to an IAM role definition in a service template\.

### Example<a name="parameter-filters.proton.cfn-policy-arns.example"></a>

With the following custom component template:

```
Resources:
  # ...
  ExamplePolicy1:
    Type: AWS::IAM::ManagedPolicy
    Properties:
      # ...
  ExamplePolicy2:
    Type: AWS::IAM::ManagedPolicy
    Properties:
      # ...

  # ...

Outputs:
  Output1:
    Description: "Example component output 1"
    Value: hello
  Output2:
    Description: "Example component output 2"
    Value: world
  PolicyArn1:
    Description: "ARN of policy 1"
    Value: !Ref ExamplePolicy1
  PolicyArn2:
    Description: "ARN of policy 2"
    Value: !Ref ExamplePolicy2
```

And the following service template:

```
Resources: 

  # ...

  TaskRole:
    Type: AWS::IAM::Role
    Properties:
      # ...
      ManagedPolicyArns:
        - !Ref BaseTaskRoleManagedPolicy
        {{ service_instance.components.default.outputs
            | proton_cfn_iam_policy_arns }}

  # Basic permissions for the task
  BaseTaskRoleManagedPolicy:
    Type: AWS::IAM::ManagedPolicy
    Properties:
      # ...
```

The rendered service template is as follows:

```
Resources: 

  # ...

  TaskRole:
    Type: AWS::IAM::Role
    Properties:
      # ...
      ManagedPolicyArns:
        - !Ref BaseTaskRoleManagedPolicy
        - arn:aws:iam::123456789012:policy/cfn-generated-policy-name-1
        - arn:aws:iam::123456789012:policy/cfn-generated-policy-name-2

  # Basic permissions for the task
  BaseTaskRoleManagedPolicy:
    Type: AWS::IAM::ManagedPolicy
    Properties:
      # ...
```

## Sanitize property values<a name="parameter-filters.proton.cfn-sanitize"></a>

**Declaration**

```
string → proton_cfn_sanitize → string
```

**Description**

This is a general purpose filter\. Use it to validate the safety of a parameter value\. The filter validates that the value either matches the regular expression `^[a-zA-Z0-9_-]*$` or is a valid Amazon Resource Name \(ARN\)\. If the value fails this validation, template rendering fails\.

### Example<a name="parameter-filters.proton.cfn-sanitize.example"></a>

With the following custom component template:

```
Resources:
  # ...
Outputs:
  Output1:
    Description: "Example of valid output"
    Value: "This-is_valid_37"
  Output2:
    Description: "Example incorrect output"
    Value: "this::is::incorrect"
  SomeArn:
    Description: "Example ARN"
    Value: arn:aws:some-service::123456789012:some-resource/resource-name
```
+ The following reference in a service template:

  ```
  # ...
    {{ service_instance.components.default.outputs.Output1
      | proton_cfn_sanitize }}
  ```

  Renders as follows:

  ```
  # ...
    This-is_valid_37
  ```
+ The following reference in a service template:

  ```
  # ...
    {{ service_instance.components.default.outputs.Output2
      | proton_cfn_sanitize }}
  ```

  Results with the following rendering error:

  ```
  Illegal character(s) detected in "this::is::incorrect". Must match regex ^[a-zA-Z0-9_-]*$ or be a valid ARN
  ```
+ The following reference in a service template:

  ```
  # ...
    {{ service_instance.components.default.outputs.SomeArn
      | proton_cfn_sanitize }}
  ```

  Renders as follows:

  ```
  # ...
    arn:aws:some-service::123456789012:some-resource/resource-name
  ```

## Provide default values for nonexistent references<a name="parameter-filters.proton.default"></a>

**Description**

The `default` filter provides a default value when a namespace reference doesn't exist\. Use it to write robust templates that can render without failure even when the parameter you refer to is missing\.

### Example<a name="parameter-filters.default.example"></a>

The following reference in a service template causes template rendering to fail if the service instance doesn't have an attached directly defined \(default\) component, or if the attached component doesn't have an output named `test`\.

```
# ...
  {{ service_instance.components.default.outputs.test }}
```

To avoid this issue, add the `default` filter\.

```
# ...
  {{ service_instance.components.default.outputs.test | default("[optional-value]") }}
```