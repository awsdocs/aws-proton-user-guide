# Service CloudFormation IaC file parameter details and examples<a name="svc-parameters"></a>

You can define and reference parameters in your service and pipeline infrastructure as code \(IaC\) files\. For a detailed description of AWS Proton parameters, parameter types, the parameter namespace, and how to use parameters in your IaC files, see [AWS Proton parameters](parameters.md)\.

## Define service parameters<a name="svc-parameters.define"></a>

You can define both input and output parameters for service IaC files\.
+ **Input parameters** – Define service instance input parameters in your [schema file](ag-schema.md)\.

  The following list includes examples of service input parameters for typical use cases\.
  + Port
  + Task size
  + Image
  + Desired count
  + Docker file
  + Unit test command

  You provide values for input parameters when you [create a service](ag-create-svc.md):
  + Use the console to fill out a schema\-based form that AWS Proton provides\.
  + Use the CLI to provide a spec that includes the values\.
+ **Output parameters** – Define service instance outputs in your service IaC files\. You can then refer to these outputs in IaC files of other resources\.

## Read parameter values in service IaC files<a name="svc-parameters.refer"></a>

You can read parameters related to the service and to other resources in service IaC files\. You read a parameter value by referencing the parameter's name in the AWS Proton parameter namespace\.
+ **Input parameters** – Read a service instance input value by referencing `service_instance.inputs.input-name`\.
+ **Resource parameters** – Read AWS Proton resource parameters by referencing names such as `service.name`, `service_instance.name`, and `environment.name`\.
+ **Output parameters** – Read outputs of other resources by referencing `environment.outputs.output-name` or `service_instance.components.default.outputs.output-name`\.

## Example service IaC file with parameters<a name="svc-parameters.example"></a>

The following example is a snippet from a service CloudFormation IaC file\. The `environment.outputs.` namespace refers to outputs from the environment IaC file\. The `service_instance.inputs.` namespace refers to service instance input parameters\. The `service_instance.name` property refers to an AWS Proton resource parameter\.

```
Resources:
  StoreServiceInstanceInputValue:
    Type: AWS::SSM::Parameter
    Properties:
      Type: String
      Value: "{{ service.name }} {{ service_instance.name }} {{ service_instance.inputs.my_sample_service_instance_required_input }} {{ service_instance.inputs.my_sample_service_instance_optional_input }} {{ environment.outputs.MySampleInputValue }} {{ environment.outputs.MyOtherSampleInputValue }}"
              #  resource parameter references               # input parameter references                                                                                                                    # output references to an environment infrastructure as code file
Outputs:
  MyServiceInstanceParameter:                                                         # output definition
    Value: !Ref StoreServiceInstanceInputValue 
  MyServiceInstanceRequiredInputValue:                                                # output definition
    Value: "{{ service_instance.inputs.my_sample_service_instance_required_input }}"  # input parameter reference
  MyServiceInstanceOptionalInputValue:                                                # output definition
    Value: "{{ service_instance.inputs.my_sample_service_instance_optional_input }}"  # input parameter reference
  MyServiceInstancesEnvironmentSampleOutputValue:                                     # output definition
    Value: "{{ environment.outputs.MySampleInputValue }}"                             # output reference to an environment IaC file
  MyServiceInstancesEnvironmentOtherSampleOutputValue:                                # output definition
    Value: "{{ environment.outputs.MyOtherSampleInputValue }}"                        # output reference to an environment IaC file
```