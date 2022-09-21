# Component AWS CloudFormation example<a name="ag-components-example-cfn"></a>

Here is a complete example of an AWS Proton directly defined component and how you can use it in an AWS Proton service\. The component provisions an Amazon Simple Storage Service \(Amazon S3\) bucket and related access policy\. The service instance can refer to this bucket and use it\. The bucket name is based on the names of the environment, service, service instance, and component, meaning that the bucket is coupled with a specific instance of the component template extending a specific service instance\. Developers can create multiple components based on this component template, to provision Amazon S3 buckets for different service instances and functional needs\.

The example covers authoring the various required AWS CloudFormation infrastructure as code \(IaC\) files and creating a required AWS Identity and Access Management \(IAM\) role\. The example groups steps by the owning people roles\.

## Administrator steps<a name="ag-components-example-cfn.admin"></a>

**To enable developers to use components with a service**

1. Create an AWS Identity and Access Management \(IAM\) role that scopes down the resources that directly defined components running in your environment can provision\. AWS Proton assumes this role later to provision directly defined components in the environment\.

   For this example, use the following policy:  
**Example directly defined component role**  

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "cloudformation:CancelUpdateStack",
           "cloudformation:CreateChangeSet",
           "cloudformation:DeleteChangeSet",
           "cloudformation:DescribeStacks",
           "cloudformation:ContinueUpdateRollback",
           "cloudformation:DetectStackResourceDrift",
           "cloudformation:DescribeStackResourceDrifts",
           "cloudformation:DescribeStackEvents",
           "cloudformation:CreateStack",
           "cloudformation:DeleteStack",
           "cloudformation:UpdateStack",
           "cloudformation:DescribeChangeSet",
           "cloudformation:ExecuteChangeSet",
           "cloudformation:ListChangeSets",
           "cloudformation:ListStackResources"
         ],
         "Resource": "arn:aws:cloudformation:*:123456789012:stack/AWSProton-*"
       },
       {
         "Effect": "Allow",
         "Action": [
           "s3:CreateBucket",
           "s3:DeleteBucket",
           "s3:GetBucket",
           "iam:CreatePolicy",
           "iam:DeletePolicy",
           "iam:GetPolicy",
           "iam:ListPolicyVersions",
           "iam:DeletePolicyVersion"
         ],
         "Resource": "*",
         "Condition": {
           "ForAnyValue:StringEquals": {
             "aws:CalledVia": "cloudformation.amazonaws.com"
           }
         }
       }
     ]
   }
   ```

1. Provide the role you created in the previous step when you create or update the environment\. In the AWS Proton console, specify a **Component role** on the **Configure environment** page\. If you're using the AWS Proton API or AWS CLI, specify the `componentRoleArn` of the [CreateEnvironment](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateEnvironment.html) or [UpdateEnvironment](https://docs.aws.amazon.com/proton/latest/APIReference/API_UpdateEnvironment.html) API actions\.

1. Create a service template that refers to a directly defined component attached to the service instance\.

   The example shows how to write a robust service template that doesn't break if a component isn't attached to the service instance\.  
**Example service CloudFormation IaC file using a component**  

   ```
   # service/instance_infrastructure/cloudformation.yaml
   
   Resources: 
     TaskDefinition:
       Type: AWS::ECS::TaskDefinition
       Properties:
         TaskRoleArn: !Ref TaskRole
         ContainerDefinitions:
           - Name: '{{service_instance.name}}'
             # ...
             {% if service_instance.components.default.outputs | length > 0 %}
             Environment:
               {{ service_instance.components.default.outputs |
                   proton_cfn_ecs_task_definition_formatted_env_vars }}
             {% endif %}
   
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

1. Create a new service template minor version that declares directly defined components as supported\.
   + **Template bundle in Amazon S3** – In the AWS Proton console, when you create a service template version, for **Supported component sources**, choose **Directly defined**\. If you're using the AWS Proton API or AWS CLI, specify `DIRECTLY_DEFINED` in the `supportedComponentSources` parameter of the [CreateServiceTemplateVersion](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateServiceTemplateVersion.html) or [UpdateServiceTemplateVersion](https://docs.aws.amazon.com/proton/latest/APIReference/API_UpdateServiceTemplateVersion.html) API actions\.
   + **Template sync** – Commit a change to your service template bundle repository, where you specify `DIRECTLY_DEFINED` as an item of `supported_component_sources:` in the `.template-registration.yaml` file in the major version directory\. For more information about this file, see [Syncing service templates](create-template-sync.md#create-template-sync-service-templates)\.

1. Publish the new service template minor version\. For more information, see [Register and publish templates](template-create.md)\.

1. Be sure to allow the `proton:CreateComponent` in the IAM role of developers that use this service template\.

## Developer steps<a name="ag-components-example-cfn.dev"></a>

**To use a directly defined component with a service instance**

1. Create a service that uses the service template version that the administrator created with component support\. Alternatively, update one of your existing service instances to use the latest template version\.

1. Write a component IaC template file that provisions an Amazon S3 bucket and a related access policy and exposes these resources as outputs\.  
**Example component CloudFormation IaC file**  

   ```
   # cloudformation.yaml
   
   # A component that defines an S3 bucket and a policy for accessing the bucket.
   Resources:
     S3Bucket:
       Type: 'AWS::S3::Bucket'
       Properties:
         BucketName: '{{environment.name}}-{{service.name}}-{{service_instance.name}}-{{component.name}}'
     S3BucketAccessPolicy:
       Type: AWS::IAM::ManagedPolicy
       Properties:
         PolicyDocument:
           Version: "2012-10-17"
           Statement:
             - Effect: Allow
               Action:
                 - 's3:Get*'
                 - 's3:List*'
                 - 's3:PutObject'
               Resource: !GetAtt S3Bucket.Arn
   Outputs:
     BucketName:
       Description: "Bucket to access"
       Value: !GetAtt S3Bucket.Arn
     BucketAccessPolicyArn:
       Value: !Ref S3BucketAccessPolicy
   ```

1. If you're using the AWS Proton API or AWS CLI, write a manifest file for the component\.  
**Example directly defined component manifest**  

   ```
   infrastructure:
     templates:
       - file: "cloudformation.yaml"
         rendering_engine: jinja
         template_language: cloudformation
   ```

1. Create a directly defined component\. AWS Proton assumes the component role that the administrator defined to provision the component\.

   In the AWS Proton console, on the [Components](https://console.aws.amazon.com/proton/#/components) page, choose **Create component**\. For **Component settings**, enter a **Component name** and an optional **Component description**\. For **Component attachment**, choose **Attach the component to a service instance\.** Select your environment, service, and service instance\. For **Component source**, choose **AWS CloudFormation**, and then choose the component IaC file\.
**Note**  
You don't need to provide a manifest—the console creates one for you\.

   If you're using the AWS Proton API or AWS CLI, use the [CreateComponent](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateComponent.html) API action\. Set a component `name` and optional `description`\. Set `environmentName`, `serviceName`, and `serviceInstanceName`\. Set `templateSource` and `manifest` to the paths of the files you created\.
**Note**  
Specifying an environment name is optional when you specify service and service instance names\. The combination of these two is unique in your AWS account, and AWS Proton can determine the environment from the service instance\.

1. Update your service instance to redeploy it\. AWS Proton uses outputs from your component in the rendered service instance template, to enable your application to use the Amazon S3 bucket that the component provisioned\.