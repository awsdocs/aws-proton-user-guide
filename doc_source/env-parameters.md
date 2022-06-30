# Environment CloudFormation IaC file parameter details and examples<a name="env-parameters"></a>

You can define and reference parameters in your environment infrastructure as code \(IaC\) files\. For a detailed description of AWS Proton parameters, parameter types, the parameter namespace, and how to use parameters in your IaC files, see [AWS Proton parameters](parameters.md)\.

## Define environment parameters<a name="env-parameters.define"></a>

You can define both input and output parameters for environment IaC files\.
+ **Input parameters** – Define environment input parameters in your [schema file](ag-schema.md)\.

  The following list includes examples of environment input parameters for typical use cases\.
  + VPC CIDR values
  + Load balancer settings
  + Database settings
  + A health check timeout

  As an administrator, you can provide values for input parameters when you [create an environment](ag-create-env.md):
  + Use the console to fill out a schema\-based form that AWS Proton provides\.
  + Use the CLI to provide a spec that includes the values\.
+ **Output parameters** – Define environment outputs in your environment IaC files\. You can then refer to these outputs in IaC files of other resources\.

## Read parameter values in environment IaC files<a name="env-parameters.refer"></a>

You can read parameters related to the environment in environment IaC files\. You read a parameter value by referencing the parameter's name in the AWS Proton parameter namespace\.
+ **Input parameters** – Read an environment input value by referencing `environment.inputs.input-name`\.
+ **Resource parameters** – Read AWS Proton resource parameters by referencing names such as `environment.name`\.

**Note**  
No output parameters of other resources are available to environment IaC files\.

## Example environment and service IaC files with parameters<a name="env-parameters.example"></a>

The following example demonstrates parameter definition and reference in an environment IaC file\. The example then shows how environment output parameters defined in the environment IaC file can be referenced in a service IaC file\.

**Example Environment CloudFormation IaC file**  
Note the following in this example:  
+ The `environment.inputs.` namespace refers to environment input parameters\.
+ The Amazon EC2 Systems Manager \(SSM\) parameter `StoreInputValue` concatenates the environment inputs\.
+ The `MyEnvParameterValue` output exposes the same input parameter concatenation as an output parameter\. Three additional output parameters also expose the input parameters individually\.
+ Six additional output parameters expose resources that the environment provisions\.

```
Resources:
  StoreInputValue:
    Type: AWS::SSM::Parameter
    Properties:
      Type: String
      Value: "{{ environment.inputs.my_sample_input }} {{ environment.inputs.my_other_sample_input}} {{ environment.inputs.another_optional_input }}"
              # input parameter references

# These output values are available to service infrastructure as code files as outputs, when given the 
# the 'environment.outputs' namespace, for example, service_instance.environment.outputs.ClusterName.
Outputs:
  MyEnvParameterValue:                                        # output definition
    Value: !GetAtt StoreInputValue.Value
  MySampleInputValue:                                         # output definition
    Value: "{{ environment.inputs.my_sample_input }}"         #   input parameter reference
  MyOtherSampleInputValue:                                    # output definition
    Value: "{{ environment.inputs.my_other_sample_input }}"   #   input parameter reference
  AnotherOptionalInputValue:                                  # output definition
    Value: "{{ environment.inputs.another_optional_input }}"  #   input parameter reference
  ClusterName:                                                # output definition
    Description: The name of the ECS cluster
    Value: !Ref 'ECSCluster'                                  #   provisioned resource
  ECSTaskExecutionRole:                                       # output definition
    Description: The ARN of the ECS role
    Value: !GetAtt 'ECSTaskExecutionRole.Arn'                 #   provisioned resource
  VpcId:                                                      # output definition
    Description: The ID of the VPC that this stack is deployed in
    Value: !Ref 'VPC'                                         #   provisioned resource
  PublicSubnetOne:                                            # output definition
    Description: Public subnet one
    Value: !Ref 'PublicSubnetOne'                             #   provisioned resource
  PublicSubnetTwo:                                            # output definition
    Description: Public subnet two
    Value: !Ref 'PublicSubnetTwo'                             #   provisioned resource
  ContainerSecurityGroup:                                     # output definition
    Description: A security group used to allow Fargate containers to receive traffic
    Value: !Ref 'ContainerSecurityGroup'                      #   provisioned resource
```

**Example Service CloudFormation IaC file**  
The `environment.outputs.` namespace refers to environment outputs from an environment IaC file\. For example, the name `environment.outputs.ClusterName` reads the value of the `ClusterName` environment output parameter\.  

```
AWSTemplateFormatVersion: '2010-09-09'
Description: Deploy a service on AWS Fargate, hosted in a public subnet, and accessible via a public load balancer.
Mappings:
  TaskSize:
    x-small:
      cpu: 256
      memory: 512
    small:
      cpu: 512
      memory: 1024
    medium:
      cpu: 1024
      memory: 2048
    large:
      cpu: 2048
      memory: 4096
    x-large:
      cpu: 4096
      memory: 8192
Resources:
  # A log group for storing the stdout logs from this service's containers
  LogGroup:
    Type: AWS::Logs::LogGroup
    Properties:
      LogGroupName: '{{service_instance.name}}' # resource parameter

  # The task definition. This is a simple metadata description of what
  # container to run, and what resource requirements it has.
  TaskDefinition:
    Type: AWS::ECS::TaskDefinition
    Properties:
      Family: '{{service_instance.name}}' # resource parameter
      Cpu: !FindInMap [TaskSize, {{service_instance.inputs.task_size}}, cpu] # input parameter
      Memory: !FindInMap [TaskSize, {{service_instance.inputs.task_size}}, memory] 
      NetworkMode: awsvpc
      RequiresCompatibilities:
        - FARGATE
      ExecutionRoleArn: '{{environment.outputs.ECSTaskExecutionRole}}'  # output reference to an environment infrastructure code file
      TaskRoleArn: !Ref "AWS::NoValue"
      ContainerDefinitions:
        - Name: '{{service_instance.name}}'  # resource parameter
          Cpu: !FindInMap [TaskSize, {{service_instance.inputs.task_size}}, cpu]
          Memory: !FindInMap [TaskSize, {{service_instance.inputs.task_size}}, memory]
          Image: '{{service_instance.inputs.image}}'
          PortMappings:
            - ContainerPort: '{{service_instance.inputs.port}}' # input parameter
          LogConfiguration:
            LogDriver: 'awslogs'
            Options:
              awslogs-group: '{{service_instance.name}}' # resource parameter
              awslogs-region: !Ref 'AWS::Region'
              awslogs-stream-prefix: '{{service_instance.name}}' # resource parameter

  # The service_instance. The service is a resource which allows you to run multiple
  # copies of a type of task, and gather up their logs and metrics, as well
  # as monitor the number of running tasks and replace any that have crashed
  Service:
    Type: AWS::ECS::Service
    DependsOn: LoadBalancerRule
    Properties:
      ServiceName: '{{service_instance.name}}'  # resource parameter
      Cluster: '{{environment.outputs.ClusterName}}' # output reference to an environment infrastructure as code file
      LaunchType: FARGATE
      DeploymentConfiguration:
        MaximumPercent: 200
        MinimumHealthyPercent: 75
      DesiredCount: '{{service_instance.inputs.desired_count}}'# input parameter
      NetworkConfiguration:
        AwsvpcConfiguration:
          AssignPublicIp: ENABLED
          SecurityGroups:
            - '{{environment.outputs.ContainerSecurityGroup}}' # output reference to an environment infrastructure as code file
          Subnets:
            - '{{environment.outputs.PublicSubnetOne}}' # output reference to an environment infrastructure as code file
            - '{{environment.outputs.PublicSubnetTwo}}' # output reference to an environment infrastructure as code file
      TaskDefinition: !Ref 'TaskDefinition'
      LoadBalancers:
        - ContainerName: '{{service_instance.name}}'  # resource parameter
          ContainerPort: '{{service_instance.inputs.port}}' # input parameter
          TargetGroupArn: !Ref 'TargetGroup'
[...]
```