--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# Create a service<a name="ug-svc-create"></a>

To deploy an application using the Proton service, as a developer, you can either use the Proton console or the AWS CLI\. With either method, you only need to provide the following inputs\.

1. The name of a Proton service template published by your platform team\.

1. A name for the service\.

1. The number of service instances you wish to deploy\.

1. A selection of environments you wish to use\.

1. A connection to your GitHub repository\.

## Service templates<a name="ug-svc-templates"></a>

Major and minor versions of service templates will be available to you\. When you use the console, you select the latest `Recommended` major and minor version of the service template\. When you use the AWS CLI and you specify only the major version of the service template, you implicitly specify the latest `Recommended` minor version of the service template for the major version you specified\.

The following describes the difference between major and minor template versions and their use\.
+ New versions of a template become `Recommended` as soon as they are approved by a member of your platform team\. This means that new services will be created using that version and you will be prompted to update existing services to the new version\.
+ Through Proton, your platform team can automatically update service instances to a new minor version of a service template\. Minor versions must be backward compatible\. 
+ Because major versions require you to provide new inputs as part of the update process, you need to update your service to a major version of its service template\. Major versions are not backward compatible\.

## Create a service<a name="svc-create-process"></a>

If you choose to use the console, you can provide your inputs directly to Proton as described in [Getting started with AWS Proton](ug-getting-started.md)\. Proton creates a YAML file from your inputs to the console\. If you want to use the AWS CLI instead, you can create a YAML formatted file in your code directory that contains the inputs, as outlined in the following sections\.

As a developer, you specify your entire service in a single file, `.aws-proton/service.yaml`, in your source code directory\. This file includes inputs for the pipeline and for service instances, as shown in the following example\.

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

To create the service, you provide the file as input on the command line as shown in the following example\.

```
aws proton --region region-id create-service --service-name "simple-svc" --branch-name "mainline" --major-version-id "1" --service-template-arn arn:aws:proton:region-id:account-id:service-template/fargate-service --repository-connection-arn arn:aws:proton:region-id:account-id --repository-id "myorg/myapp" --spec file://spec.yaml
```

```
{
    "service": {
        "arn": "arn:aws:proton:region-id:123456789012:service/simple-svc",
        "createdAt": "2020-11-18T02:13:20.216000+00:00",
        "lastModifiedAt": "2020-11-18T02:13:20.216000+00:00",
        "serviceName": "simple-svc",
        "serviceTemplateArn": "arn:aws:proton:region-id:123456789012:service-template/fargate-service",
        "status": "CREATE_STARTED"
    }
}
```

This command creates the pipeline and the individual service instances\. You can view the instances using the AWS CLI or the console\.