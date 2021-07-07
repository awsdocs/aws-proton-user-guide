# Create a service<a name="ug-svc-create"></a>

To deploy an application with AWS Proton, as a developer, you can either use the AWS Proton console or the AWS CLI\. With either method, you only need to provide the following inputs\.

1. The name of an AWS Proton service template that's published by your platform team\.

1. A name for the service\.

1. The number of service instances that you want to deploy\.

1. A selection of environments that you want to use\.

1. A connection to your code repository if you're using a service template that includes a service pipeline \(optional\)\.

## Service templates<a name="ug-svc-templates"></a>

Both major and minor versions of service templates are available\. When you use the console, you select the latest `Recommended` major and minor version of the service template\. When you use the AWS CLI and you specify only the major version of the service template, you implicitly specify the latest `Recommended` minor version of the service template for the major version you specified\.

The following describes the difference between major and minor template versions and their use\.
+ New versions of a template become `Recommended` as soon as they're approved by a member of your platform team\. This means that new services are created using that version, and you're prompted to update existing services to the new version\.
+ Through AWS Proton, your platform team can automatically update service instances to a new minor version of a service template\. Minor versions must be backward compatible\. 
+ Because major versions require you to provide new inputs as part of the update process, you need to update your service to a major version of its service template\. Major versions *aren't* backward compatible\.

## Create a service<a name="svc-create-process"></a>

When you create a service, you can choose from two different types of service templates as shown in the following list\.
+ A service template that includes a service pipeline \(default\)\.
+ A service template that *doesn't* include a service pipeline\.

**Create a service using the console\.**

When you use the console, you can provide your service inputs directly to AWS Proton as described in [Getting started with AWS Proton](ug-getting-started.md)\. AWS Proton creates a YAML file from your inputs to the console\. It is available for download in the service detail page **Actions** drop\-down, after your service is created\. When you *don't* want to use an AWS Proton service pipeline, choose a template marked with *Excludes pipeline*\.

**Create a service with an AWS Proton service pipeline using the CLI\.**

When you use the AWS CLI, you specify service inputs in a YAML formatted file, `.aws-proton/service.yaml`, located in your source code directory\. If you want to use a service template that has `pipelineProvisioning: "CUSTOMER_MANAGED"`, *don’t* include the `pipeline:` section in your spec and *don’t* include `--repository-connection-arn`, `--repository-id`, and `--branch-name` parameters in your `create-service` command\. Use the `get-service-template` command to see if `pipelineProvisioning` is set to `"CUSTOMER_MANAGED"`\.

The following example command and response shows how to use the CLI to view the schema `required` and `optional` parameters that you provide values for in your spec file\. The schema is found in the response `schema` output\.

Command:

```
aws proton get-service-template-version --template-name "fargate-service" --major-version "1" --minor-version "0"
```

Response:

```
{
    "serviceTemplateMinorVersion": {
        "arn": "arn:aws:proton:us-west-2:123456789012:service-template/fargate-service:1.0",
        "compatibleEnvironmentTemplates": [
            {
                "majorVersion": "1",
                "templateName": "simple-env"
            }
        ],
        "createdAt": 1613087544.189,
        "description": "Version 1",
        "lastModifiedAt": 1613087546.101,
        "majorVersion": "1",
        "minorVersion": "0",
        "schema": "schema:\n  format:\n    openapi: \"3.0.0\"\n  pipeline_input_type: \"MyPipelineInputType\"\n  service_input_type: \"MyServiceInstanceInputType\"\n\n  types:\n    MyPipelineInputType:\n      type: object\n      description: \"Pipeline input properties\"\n      required:\n        - my_sample_pipeline_required_input\n      properties:\n        my_sample_pipeline_optional_input:\n          type: string\n          description: \"This is a sample input\"\n          default: \"hello world\"\n        my_sample_pipeline_required_input:\n          type: string\n          description: \"Another sample input\"\n\n    MyServiceInstanceInputType:\n      type: object\n      description: \"Service instance input properties\"\n      required:\n        - my_sample_service_instance_required_input\n      properties:\n        my_sample_service_instance_optional_input:\n          type: string\n          description: \"This is a sample input\"\n          default: \"hello world\"\n        my_sample_service_instance_required_input:\n          type: string\n          description: \"Another sample input\"",
        "status": "DRAFT",
        "statusMessage": "",
        "templateName": "fargate-service"
    }
}
```

The formatted schema from the response is shown in the next example, for your convenience\.

Schema:

```
schema:
  format:
    openapi: "3.0.0"
  pipeline_input_type: "MyPipelineInputType"
  service_input_type: "MyServiceInstanceInputType"
  types:
    MyPipelineInputType:
      type: object
      description: "Pipeline input properties"
      required:
        - my_sample_pipeline_required_input
      properties:
        my_sample_pipeline_optional_input:          # parameter
          type: string
          description: "This is a sample input"
          default: "hello world"
        my_sample_pipeline_required_input:          # parameter
          type: string
          description: "Another sample input"
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

The following example spec includes parameter values for a service pipeline and one instance, based on the schema file\.

Spec:

```
proton: ServiceSpec

pipeline:
  my_sample_pipeline_required_input: "hello"
  my_sample_pipeline_optional_input: "bye"

instances:
  - name: "acme-network-dev"
    environment: "ENV_NAME"
    spec:
      my_sample_service_instance_required_input: "hi"
      my_sample_service_instance_optional_input: "ho"
```

Create a service as defined by a service template by specifying the name, repository connection ARN, repository ID, repository branch, template name, spec, the major and minor versions, description \(optional\) and tags \(optional\) as shown in the following command and response\.

Command:

```
aws proton create-service --name "simple-svc" --branch-name "mainline" --major-version "1" --template-name "fargate-service" --repository-connection-arn arn:aws:proton:region-id:account-id --repository-id "myorg/myapp" --spec "file://spec.yaml"
```

Response:

```
{
    "service": {
        "arn": "arn:aws:proton:region-id:123456789012:service/MySimpleService",
        "createdAt": "2020-11-18T19:50:27.460000+00:00",
        "lastModifiedAt": "2020-11-18T19:50:27.460000+00:00",
        "name": "MySimpleService",
        "status": "CREATE_IN_PROGRESS",
        "templateName": "fargate-service"
    }
}
```

**Create a service *without* an AWS Proton service pipeline by using the CLI\.**

The following example spec *doesn't* include inputs for a service pipeline\.

Spec:

```
proton: ServiceSpec

instances:
  - name: "acme-network-dev"
    environment: "ENV_NAME"
    spec:
      my_sample_service_instance_required_input: "hi"
      my_sample_service_instance_optional_input: "ho"
```

To create a service *without* an AWS Proton service pipeline, you provide the path to a `spec.yaml` and you *don't* include repository parameters as shown in the following example command and response\.

Command:

```
aws proton create-service --name "MySimpleServiceNoPipeline" --major-version "1" --template-name "fargate-service" --spec "file://spec-no-pipeline.yaml"
```

Response:

```
{
    "service": {
        "arn": "arn:aws:proton:region-id:123456789012:service/MySimpleServiceNoPipeline",
        "createdAt": "2020-11-18T19:50:27.460000+00:00",
        "lastModifiedAt": "2020-11-18T19:50:27.460000+00:00",
        "name": "MySimpleServiceNoPipeline",
        "templateName": "fargate-service-no-pipeline",
        "status": "CREATE_IN_PROGRESS"
    }
}
```