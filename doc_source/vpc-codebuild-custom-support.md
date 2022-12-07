# CodeBuild provisioning custom Amazon VPC support<a name="vpc-codebuild-custom-support"></a>

AWS Proton CodeBuild Provisioning executes arbitrary customer\-supplied CLI commands in a CodeBuild project located in the AWS Proton Environment account\. These commands typically manage resource susing an Infrastructure as Code \(IaC\) tool, such as CDK\. If you have resources in a Amazon VPC, CodeBuild may not be able to access them\. To enable this, CodeBuild supports the ability to run within a specific Amazon VPC\. A few example uses cases include:
+ Retrieve dependencies from self\-hosted, internal artifact repositories, such as `PyPI` for Python, `Maven` for Java, and `npm` for Node\.js
+ CodeBuild needs to access a Jenkins server in a particular Amazon VPC to register a pipeline\.
+ Access objects in an Amazon S3 bucket configured to allow access through an Amazon VPC endpoint only\.
+ Run integration tests from your build against data in an Amazon RDS database that's isolated on a private subnet\.

For more information, see [CodeBuild and VPC documentation](https://docs.aws.amazon.com/codebuild/latest/userguide/vpc-support.html#use-cases)\.

If you want CodeBuild Provisioning to run in a custom VPC, AWS Proton provides a straightforward solution\. First, you must add the VPC ID, subnets, and security groups to the environment template\. Next, you enter those values into the environment spec\. This will result in a CodeBuild project being created for you that targets a given VPC\.

## Updating the Environment Template<a name="vpc-codebuild-update-environment-template"></a>

**Schema**  
The VPC ID, subnets, and security groups need to be added to the template schema so they can exist in the environment spec\.

An example `schema.yaml`:

```
schema:
  format:
    openapi: "3.0.0"
  environment_input_type: "EnvironmentInputType"
  types:
    EnvironmentInputType:
      type: object
      properties:
        codebuild_vpc_id:
          type: string
        codebuild_subnets:
          type: array
          items:
            type: string
        codebuild_security_groups:
          type: array
          items:
            type: string
```

This adds three new properties that will be used by the manifest:
+ `codebuild_vpc_id`
+ `codebuild_subnets`
+ `codebuild_security_groups`

**Manifest**  
To configure Amazon VPC settings in CodeBuild, an optional property called `project_properties` is available in the template manifest\. Contents of `project_properties` are added to the AWS CloudFormation stack that creates the CodeBuild project\. This makes it possible to add not only [Amazon VPC AWS CloudFormation properties](https://docs.aws.amazon.com/#cfn-codebuild-project-vpcconfig), but also any supported [CodeBuild CloudFormation property](AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html), such as build timeout\. The same data provided to `proton-input.json` is made available to the values of `project_properties`\.

Add this section to your `manifest.yaml`:

```
project_properties:
          VpcConfig:
            VpcId: "{{ environment.inputs.codebuild_vpc_id }}"
            Subnets: "{{ environment.inputs.codebuild_subnets }}"
            SecurityGroupIds: "{{ environment.inputs.codebuild_security_groups }}"
```

The following is what the resulting `manifest.yaml` may look like:

```
infrastructure:
  templates:
    - rendering_engine: codebuild
      settings:
        image: aws/codebuild/amazonlinux2-x86_64-standard:4.0
        runtimes:
          nodejs: 16
        provision:
          - npm install
          - npm run build
          - npm run cdk bootstrap
          - npm run cdk deploy -- --require-approval never
        deprovision:
          - npm install
          - npm run build
          - npm run cdk destroy -- --force
        project_properties:
          VpcConfig:
            VpcId: "{{ environment.inputs.codebuild_vpc_id }}"
            Subnets: "{{ environment.inputs.codebuild_subnets }}"
            SecurityGroupIds: "{{ environment.inputs.codebuild_security_groups }}"
```

**Creating the environment**  
When you create an environment with your CodeBuild Provisioning VPC\-enabled template, you must provide the Amazon VPC ID, subnets, and security groups\.

To get a list of all Amazon VPC IDs in your Region, run the following command:

```
aws ec2 describe-vpcs
```

To get a list of all the subnet IDs, run:

```
aws ec2 describe-subnets --filters "Name=vpc-id,Values=vpc-id"
```

**Important**  
Only include private subnets\. CodeBuild will fail if you provide public subnets\. Public subnets have a default route to an [Internet Gateway](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Internet_Gateway.html), while private subnets don't\.

Run the following command to obtain the security group IDs\. These IDs can also be obtained through the AWS Management Console:

```
aws ec2 describe-security-groups --filters "Name=vpc-id,Values=vpc-id"
```

The values will resemble:

```
vpc-id: vpc-045ch35y28dec3a05
subnets:
  - subnet-04029a82e6ae46968
  - subnet-0f500a9294fc5f26a
 security-groups:
  - sg-03bc4c4ce32d67e8d
```

**Ensuring CodeBuild permissions**  
Amazon VPC support requires certain permissions, such as the ability to create an Elastic Network Interface\.

If the environment is being created in the console, enter these values during the environment creation wizard\. If you want to programmatically create the environment, your `spec.yaml` looks like the following:

```
proton: EnvironmentSpec

spec:
  codebuild_vpc_id: vpc-045ch35y28dec3a05
  codebuild_subnets:
    - subnet-04029a82e6ae46968
    - subnet-0f500a9294fc5f26a
  codebuild_security_groups:
    - sg-03bc4c4ce32d67e8d
```