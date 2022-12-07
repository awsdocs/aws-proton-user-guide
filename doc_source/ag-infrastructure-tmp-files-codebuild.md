# CodeBuild provisioning template bundle<a name="ag-infrastructure-tmp-files-codebuild"></a>

With CodeBuild provisioning, instead of using IaC templates to render IaC files and run them using an IaC provisioning engine, AWS Proton simply runs your shell commands\. To do that, AWS Proton creates an AWS CodeBuild project for the environment, in the environment account, and starts a job to run your commands for each AWS Proton resource creation or update\. When you author a template bundle, you provide a manifest that specifies infrastructure provisioning and deprovisioning commands, and any programs, scripts, and other files that these commands may need\. Your commands can read inputs that AWS Proton provides, and are responsible for provisioning or deprovisioning infrastructure and generating output values\.

The manifest also specifies how AWS Proton should render the input file that your code can input and get input values from\. It can be rendered into JSON or HCL\. For more information about input parameters, see [CodeBuild provisioning parameter details and examples](parameters-codebuild.md)\. For more information about manifest files, see [Wrap up template files for AWS Proton](ag-wrap-up.md)\.

**Note**  
You can use CodeBuild provisioning with environments and services\. At this time you can't provision components this way\.

## Example: using the AWS CDK with CodeBuild provisioning<a name="ag-infrastructure-tmp-files-codebuild.example"></a>

As an example to using CodeBuild provisioning, you can include code that uses the AWS Cloud Development Kit \(AWS CDK\) to provision \(*deploy*\) and deprovision \(*destroy*\) AWS resources, and a manifest that installs the CDK and runs your CDK code\.

The following sections list example files you can include in a CodeBuild provisioning template bundle that provisions an environment using the AWS CDK\.

### Manifest<a name="ag-infrastructure-tmp-files-codebuild.example.manifest"></a>

The following manifest file specifies CodeBuild provisioning, and includes the commands necessary to install and use the AWS CDK, output file processing, and reporting outputs back to AWS Proton\.

**Example infrastructure/manifest\.yaml**  

```
infrastructure:
  templates:
    - rendering_engine: codebuild
      codebuild:
        image: aws/codebuild/amazonlinux2-x86_64-standard:4.0
        runtimes:
          nodejs: 16
        provision:
          - npm install
          - npm run build
          - npm run cdk bootstrap
          - npm run cdk deploy -- --require-approval never --outputs-file proton-outputs.json
          - jq 'to_entries | map_values(.value) | add | to_entries | map({key:.key, valueString:.value})' < proton-outputs.json > outputs.json
          - aws proton notify-resource-deployment-status-change --resource-arn $RESOURCE_ARN --status IN_PROGRESS --outputs file://./outputs.json
        deprovisioning:
          - npm install
          - npm run build
          - npm run cdk destroy
        project_properties:
          VpcConfig:
            VpcId: "{{ environment.inputs.codebuild_vpc_id }}"
            Subnets: "{{ environment.inputs.codebuild_subnets }}"
            SecurityGroupIds: "{{ environment.inputs.codebuild_security_groups }}"
```

### Schema<a name="ag-infrastructure-tmp-files-codebuild.example.schema"></a>

The following schema file defines parameters for the environment\. Your AWS CDK code can refer to values of these parameters during deployment\.

**Example schema/schema\.yaml**  

```
schema:
  format:
    openapi: "3.0.0"
  environment_input_type: "MyEnvironmentInputType"
  types:
    MyEnvironmentInputType:
      type: object
      description: "Input properties for my environment"
      properties:
        my_sample_input:
          type: string
          description: "This is a sample input"
          default: "hello world"
        my_other_sample_input:
          type: string
          description: "Another sample input"
      required:
        - my_other_sample_input
```

### AWS CDK files<a name="ag-infrastructure-tmp-files-codebuild.example.cdkcode"></a>

The following files are an example to a Node\.js CDK project\.

**Example infrastructure/package\.json**  

```
{
  "name": "ProtonEnvironment",
  "version": "0.1.0",
  "bin": {
    "ProtonEnvironmente": "bin/ProtonEnvironment.js"
  },
  "scripts": {
    "build": "tsc",
    "watch": "tsc -w",
    "test": "jest",
    "cdk": "cdk"
  },
  "devDependencies": {
    "@types/jest": "^28.1.7",
    "@types/node": "18.7.6",
    "jest": "^28.1.3",
    "ts-jest": "^28.0.8",
    "aws-cdk": "2.37.1",
    "ts-node": "^10.9.1",
    "typescript": "~4.7.4"
  },
  "dependencies": {
    "aws-cdk-lib": "2.37.1",
    "constructs": "^10.1.77",
    "source-map-support": "^0.5.21"
  }
}
```

**Example infrastructure/tsconfig\.json**  

```
{
  "compilerOptions": {
    "target": "ES2018",
    "module": "commonjs",
    "lib": [
      "es2018"
    ],
    "declaration": true,
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "noImplicitThis": true,
    "alwaysStrict": true,
    "noUnusedLocals": false,
    "noUnusedParameters": false,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": false,
    "inlineSourceMap": true,
    "inlineSources": true,
    "experimentalDecorators": true,
    "strictPropertyInitialization": false,
    "resolveJsonModule": true,
    "esModuleInterop": true,
    "typeRoots": [
      "./node_modules/@types"
    ]
  },
  "exclude": [
    "node_modules",
    "cdk.out"
  ]
}
```

**Example infrastructure/cdk\.json**  

```
{
  "app": "npx ts-node --prefer-ts-exts bin/ProtonEnvironment.ts",
  "outputsFile": "proton-outputs.json",
  "watch": {
    "include": [
      "**"
    ],
    "exclude": [
      "README.md",
      "cdk*.json",
      "**/*.d.ts",
      "**/*.js",
      "tsconfig.json",
      "package*.json",
      "yarn.lock",
      "node_modules",
      "test"
    ]
  },
  "context": {
    "@aws-cdk/aws-apigateway:usagePlanKeyOrderInsensitiveId": true,
    "@aws-cdk/core:stackRelativeExports": true,
    "@aws-cdk/aws-rds:lowercaseDbIdentifier": true,
    "@aws-cdk/aws-lambda:recognizeVersionProps": true,
    "@aws-cdk/aws-cloudfront:defaultSecurityPolicyTLSv1.2_2021": true,
    "@aws-cdk-containers/ecs-service-extensions:enableDefaultLogDriver": true,
    "@aws-cdk/aws-ec2:uniqueImdsv2TemplateName": true,
    "@aws-cdk/core:target-partitions": [
      "aws",
      "aws-cn"
    ]
  }
}
```

**Example infrastructure/bin/ProtonEnvironment\.ts**  

```
#!/usr/bin/env node
import 'source-map-support/register';
import * as cdk from 'aws-cdk-lib';
import { ProtonEnvironmentStack } from '../lib/ProtonEnvironmentStack';

const app = new cdk.App();
new ProtonEnvironmentStack(app, 'ProtonEnvironmentStack', {});
```

**Example infrastructure/lib/ProtonEnvironmentStack\.ts**  

```
import { Stack, StackProps } from 'aws-cdk-lib';
import { Construct } from 'constructs';
import * as cdk from 'aws-cdk-lib';
import * as ssm from 'aws-cdk-lib/aws-ssm';
import input from '../proton-input.json';

export class ProtonEnvironmentStack extends Stack {
  constructor(scope: Construct, id: string, props?: StackProps) {
    super(scope, id, { ...props, stackName: process.env.STACK_NAME });

    const ssmParam = new ssm.StringParameter(this, "ssmParam", {
      stringValue: input.environment.inputs.my_sample_input,
      parameterName: `${process.env.STACK_NAME}-Param`,
      tier: ssm.ParameterTier.STANDARD
    })

    new cdk.CfnOutput(this, 'ssmParamOutput', {
      value: ssmParam.parameterName,
      description: 'The name of the ssm parameter',
      exportName: `${process.env.STACK_NAME}-Param`
    });
  }
}
```

### Rendered input file<a name="ag-infrastructure-tmp-files-codebuild.example.manifest"></a>

When you create an environment using a CodeBuild\-based provisioning template, AWS Proton renders an input file with [input parameter values](https://docs.aws.amazon.com/proton/latest/userguide/parameters.html) that you provided\. Your code can refer to these values\. The following file is an example to a rendered input file\.

**Example infrastructure/proton\-input\.json**  

```
{
  "environment": {
    "name": "myenv",
    "inputs": {
      "my_sample_input": "10.0.0.0/16",
      "my_other_sample_input": "11.0.0.0/16"
    }
  }
}
```