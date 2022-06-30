# View template data<a name="template-view"></a>

You can view lists of templates with details and view individual templates with detail data by using the [AWS Proton console](https://console.aws.amazon.com/proton/) and AWS CLI\.

*Customer managed* environment template data includes the `provisioned` parameter with the value `CUSTOMER_MANAGED`\.

If a service template *doesn't* include a service pipeline, the service template data includes the `pipelineProvisioning` parameter with the value `CUSTOMER_MANAGED`\.

For more information, see [Register and publish templates](template-create.md)\.

You can use the console or the AWS CLI to list and view template data\.

------
#### [ AWS Management Console ]

**Use the console to list and view templates\.**

1. To view a list of templates, choose **\(Environment or Service\) templates**\.

1. To view detail data choose the name of a template\.

   View the detail data of the template, a list of the major and minor versions of the template, a list of the AWS Proton resources that were deployed using template versions and template tags\.

   The recommended major version and minor version is labeled as **Recommended**\.

------
#### [ AWS CLI ]

**Use the AWS CLI to list and view templates\.**

Run the following command:

```
$ aws proton get-environment-template-version \
    --template-name "simple-env" \
    --major-version "1" \
    --minor-version "0"
```

Response:

```
{
    "environmentTemplateVersion": {
        "arn": "arn:aws:proton:region-id:123456789012:environment-template/simple-env:1.0",
        "createdAt": "2020-11-10T18:35:08.293000+00:00",
        "description": "Version 1",
        "lastModifiedAt": "2020-11-10T18:35:11.162000+00:00",
        "majorVersion": "1",
        "minorVersion": "0",
        "recommendedMinorVersion": "0",
        "schema": "schema:\n  format:\n    openapi: \"3.0.0\"\n  environment_input_type: \"MyEnvironmentInputType\"\n  types:\n    MyEnvironmentInputType:\n      type: object\n      description: \"Input properties for my environment\"\n      properties:\n        my_sample_input:\n          type: string\n          description: \"This is a sample input\"\n          default: \"hello world\"\n        my_other_sample_input:\n          type: string\n          description: \"Another sample input\"\n      required:\n        - my_other_sample_input\n",
        "status": "DRAFT",
        "statusMessage": "",
        "templateName": "simple-env"
    }
}
```

Run the following command:

```
$ aws proton list-environment-templates
```

Response:

```
{
    "templates": [
        {
            "arn": "arn:aws:proton:region-id:123456789012:environment-template/simple-env-3",
            "createdAt": "2020-11-10T18:35:05.763000+00:00",
            "description": "VPC with Public Access",
            "displayName": "VPC",
            "lastModifiedAt": "2020-11-10T18:35:05.763000+00:00",
            "name": "simple-env-3",
            "recommendedVersion": "1.0"            
        },
        {
            "arn": "arn:aws:proton:region-id:123456789012:environment-template/simple-env-1",
            "createdAt": "2020-11-10T00:14:06.881000+00:00",
            "description": "Some SSM Parameters",
            "displayName": "simple-env-1",
            "lastModifiedAt": "2020-11-10T00:14:06.881000+00:00",
            "name": "simple-env-1",
            "recommendedVersion": "1.0"           
        }
    ]
}
```

View a minor version of a service template\.

Run the following command:

```
$ aws proton get-service-template-version \
    --template-name "fargate-service" \
    --major-version "1" \
    --minor-version "0"
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
        "createdAt": "2020-11-11T23:02:57.912000+00:00",
        "description": "Version 1",
        "lastModifiedAt": "2020-11-11T23:02:57.912000+00:00",
        "majorVersion": "1",
        "minorVersion": "0",
        "schema": "schema:\n  format:\n    openapi: \"3.0.0\"\n  pipeline_input_type: \"MyPipelineInputType\"\n  service_input_type: \"MyServiceInstanceInputType\"\n\n  types:\n    MyPipelineInputType:\n      type: object\n      description: \"Pipeline input properties\"\n      required:\n        - my_sample_pipeline_required_input\n      properties:\n        my_sample_pipeline_optional_input:\n          type: string\n          description: \"This is a sample input\"\n          default: \"hello world\"\n        my_sample_pipeline_required_input:\n          type: string\n          description: \"Another sample input\"\n\n    MyServiceInstanceInputType:\n      type: object\n      description: \"Service instance input properties\"\n      required:\n        - my_sample_service_instance_required_input\n      properties:\n        my_sample_service_instance_optional_input:\n          type: string\n          description: \"This is a sample input\"\n          default: \"hello world\"\n        my_sample_service_instance_required_input:\n          type: string\n          description: \"Another sample input\"",
        "status": "DRAFT",
        "statusMessage": "",
        "templateName": "fargate-service"
    }
}
```

View a service template without a service pipeline as shown in the next example command and response\.

Run the following command:

```
$ aws proton get-service-template \
    --name "simple-svc-template-cli"
```

Response:

```
{
    "serviceTemplate": {
        "arn": "arn:aws:proton:region-id:123456789012:service-template/simple-svc-template-cli",
        "createdAt": "2021-02-18T15:38:57.949000+00:00",
        "displayName": "simple-svc-template-cli",
        "lastModifiedAt": "2021-02-18T15:38:57.949000+00:00",
        "status": "DRAFT",
        "name": "simple-svc-template-cli",
        "pipelineProvisioning": "CUSTOMER_MANAGED"
    }
}
```

------