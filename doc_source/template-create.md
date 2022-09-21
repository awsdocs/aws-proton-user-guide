# Register and publish templates<a name="template-create"></a>

You can register and publish environment and service templates with AWS Proton, as described in the following sections\.

You can create a new version of a template with the console or AWS CLI\.

Alternatively, you can use the console or AWS CLI to create a template and configure a [configure a template sync](ag-template-sync-configs.md) for it\. This configuration lets AWS Proton sync from template bundles located in registered git repositories that you have defined\. Whenever a commit is pushed to your repository that changes one of your template bundles, a new minor or major version of your template is created, if the version doesn’t already exist\. To learn more about template sync configuration prerequisites and requirements, see [Template sync configurations](ag-template-sync-configs.md)\.

## Register and publish environment templates<a name="env-template-v1"></a>

You can register and publish the following types of environment templates\.
+ Register and publish a *standard* environment template that AWS Proton uses to deploy and manage environment infrastructure\.
+ Register and publish a *customer managed* environment template that AWS Proton uses to connect to your existing provisioned infrastructure that you manage\. AWS Proton *doesn't* manage your existing provisioned infrastructure\.

**Important**  
As an administrator, ensure that your provisioned and managed infrastructure and all output parameters are compatible with associated *customer managed* environment templates\. AWS Proton can't account for changes on your behalf because these changes aren't visible to AWS Proton\. Inconsistencies result in failures\.

You can use the console or the AWS CLI to register and publish an environment template\.

------
#### [ AWS Management Console ]

**Use the console to register and publish a new environment template\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environment templates**\.

1. Choose **Create environment template**\.

1. In the **Create environment template** page, in the **Template options** section, choose one of the two available template options\.
   + **Create a template for provisioning new environments**\.
   + **Create a template to use provisioned infrastructure that you manage**\.

1. If you chose **Create a template for provisioning new environments**, in the **Template bundle source** section, choose one of the three available template bundle source options\. To learn more about requirements and prerequisites for syncing templates, see [Template sync configurations](ag-template-sync-configs.md)\.
   + **Use one of our sample template bundles**\.
   + **Use your own template bundle**\.
   + **[Sync templates from Git](ag-template-sync-configs.md)**\.

1. 

**Provide a path to a template bundle\.**

   1. If you chose **Use one of our sample template bundles**:

      In the **Sample template bundle** section, select a sample template bundle\.

   1. If you chose **Sync templates from Git**, in the **Source code** section:

      1. Select the repository for your template sync configuration\.

      1. Enter the name of the repository branch to sync from\.

      1. \(Optional\) Enter name of a directory to limit the search for your template bundle\.

   1. Otherwise, in the **S3 bundle location** section, provide a path to your template bundle\.

1. In the **Template details** section\.

   1. Enter a **Template name**\.

   1. \(Optional\) Enter a **Template display name**\.

   1. \(Optional\) Enter a **Template description** for the environment template\.

1. \(Optional\) Check the check box for **Customize encryption settings \(advanced\)** in the **Encryption settings** section to provide your own encryption key\.

1. \(Optional\) In the **Tags** section, choose **Add new tag** and enter a key and value to create a customer managed tag\.

1. Choose **Create Environment template**\.

   You're now on a new page that displays the status and details for your new environment template\. These details include a list of AWS and customer managed tags\. AWS Proton automatically generates AWS managed tags for you when you create AWS Proton resources\. For more information, see [AWS Proton resources and tagging](resources.md)\.

1. The status of a new environment template status starts in the **Draft** state\. You and others with `proton:CreateEnvironment` permissions can view and access it\. Follow the next step to make the template available to others\.

1. In the **Template versions** section, choose the radio button to the left of the minor version of the template you just created \(1\.0\)\. As an alternative, you can choose **Publish** in the info alert and skip the next step\.

1. In the **Template versions** section, choose **Publish**\.

1. The template status changes to **Published**\. Because it's the latest version of the template, it's the **Recommended** version\.

1. In the navigation pane, select **Environment templates** to view a list of your environment templates and details\.

**Use the console to register new major and minor versions of an environment template\.**

For more information, see [Versioned templates](ag-template-versions.md)\.

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environment Templates**\.

1. In the list of environment templates, choose the name of the environment template that you want to create a major or minor version for\.

1. In the environment template detail view, choose **Create new version** in the **Template versions** section\.

1. In the **Create a new environment template version** page, in the **Template bundle source** section, choose one of the two available template bundle source options\.
   + **Use one of our sample template bundles**\.
   + **Use your own template bundle**\.

1. Provide a path to the selected template bundle\.
   + If you chose **Use one of our sample template bundles**, in the **Sample template bundle** section, select a sample template bundle\.
   + If you chose **Use your own template bundle**, in the **S3 bundle location** section, choose the path to your template bundle\.

1. In the **Template details** section\.

   1. \(Optional\) Enter a **Template display name**\.

   1. \(Optional\) Enter a **Template description** for the service template\.

1. In the **Template details** section, choose one of the following options\.
   + To create a minor version, keep the check box **Check to create a new major version** empty\.
   + To create a major version, check the check box **Check to create a new major version**\.

1. Continue through the console steps to create the new minor or major version and choose **Create new version**\.

------
#### [ AWS CLI ]

**Use the CLI to register and publish a new environment template as shown in the following steps\.**

1. Create a *standard* OR *customer managed* environment template by specifying the region, name, display name \(optional\), and description \(optional\)\.

   1. Create a *standard* environment template\.

      Run the following command:

      ```
      $ aws proton create-environment-template \
          --name "simple-env" \
          --display-name "Fargate" \
          --description "VPC with public access"
      ```

      Response:

      ```
      {
          "environmentTemplate": {
              "arn": "arn:aws:proton:region-id:123456789012:environment-template/simple-env",
              "createdAt": "2020-11-11T23:02:45.336000+00:00",
              "description": "VPC with public access",
              "displayName": "VPC",
              "lastModifiedAt": "2020-11-11T23:02:45.336000+00:00",
              "name": "simple-env"
          }
      }
      ```

   1. Create a *customer managed* environment template by adding the `provisioning` parameter with value `CUSTOMER_MANAGED`\.

      Run the following command:

      ```
      $ aws proton create-environment-template \
          --name "simple-env" \
          --display-name "Fargate" \
          --description "VPC with public access" \
          --provisioning "CUSTOMER_MANAGED"
      ```

      Response:

      ```
      {
          "environmentTemplate": {
              "arn": "arn:aws:proton:region-id:123456789012:environment-template/simple-env",
              "createdAt": "2020-11-11T23:02:45.336000+00:00",
              "description": "VPC with public access",
              "displayName": "VPC",
              "lastModifiedAt": "2020-11-11T23:02:45.336000+00:00",
              "name": "simple-env",
              "provisioning": "CUSTOMER_MANAGED"
          }
      }
      ```

1. 

**Create a minor version 0 of major version 1 of the environment template**

   This and the remaining steps are the same for both the *standard* and *customer managed* environment templates\.

   Include the template name, major version, and the S3 bucket name and key for the bucket that contains your environment template bundle\.

   Run the following command:

   ```
   $ aws proton create-environment-template-version \
       --template-name "simple-env" \
       --description "Version 1" \
       --source s3="{bucket=your_s3_bucket, key=your_s3_key}"
   ```

   Response:

   ```
   {
       "environmentTemplateVersion": {
           "arn": "arn:aws:proton:region-id:123456789012:environment-template/simple-env:1.0",
           "createdAt": "2020-11-11T23:02:47.763000+00:00",
           "description": "Version 1",
           "lastModifiedAt": "2020-11-11T23:02:47.763000+00:00",
           "majorVersion": "1",
           "minorVersion": "0",
           "status": "REGISTRATION_IN_PROGRESS",
           "templateName": "simple-env"
       }
   }
   ```

1. Use the get command to check the registrations status\.

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
       "environment": {
           "arn": "arn:aws:proton:region-id:123456789012:environment-template/simple-env:1.0",
           "createdAt": "2020-11-11T23:02:47.763000+00:00",
           "description": "Version 1",
           "lastModifiedAt": "2020-11-11T23:02:47.763000+00:00",
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

1. Publish of minor version 0 of major version 1 of the environment template by providing the template name and the major and minor version\. This version is the `Recommended` version\.

   Run the following command:

   ```
   $ aws proton update-environment-template-version \
       --template-name "simple-env" \
       --major-version "1" \
       --minor-version "0" \
       --status "PUBLISHED"
   ```

   Response:

   ```
   {
       "environmentTemplateVersion": {
           "arn": "arn:aws:proton:region-id:123456789012:environment-template/simple-env:1.0",
           "createdAt": "2020-11-11T23:02:47.763000+00:00",
           "description": "Version 1",
           "lastModifiedAt": "2020-11-11T23:02:54.610000+00:00",
           "majorVersion": "1",
           "minorVersion": "0",
           "recommendedMinorVersion": "0",
           "schema": "schema:\n  format:\n    openapi: \"3.0.0\"\n  environment_input_type: \"MyEnvironmentInputType\"\n  types:\n    MyEnvironmentInputType:\n      type: object\n      description: \"Input properties for my environment\"\n      properties:\n        my_sample_input:\n          type: string\n          description: \"This is a sample input\"\n          default: \"hello world\"\n        my_other_sample_input:\n          type: string\n          description: \"Another sample input\"\n      required:\n        - my_other_sample_input\n",
           "status": "PUBLISHED",
           "statusMessage": "",
           "templateName": "simple-env"
       }
   }
   ```

After creating a new template using the AWS CLI, you can view a list of AWS and customer managed tags\. AWS Proton automatically generates AWS managed tags for you\. You can also modify and create customer managed tags using the AWS CLI\. For more information, see [AWS Proton resources and tagging](resources.md)\.

Run the following command:

```
$ aws proton list-tags-for-resource \
    --resource-arn "arn:aws:proton:region-id:123456789012:environment-template/simple-env"
```

------

## Register and publish service templates<a name="svc-template-v1"></a>

When you create a service template version, you specify a list of compatible environment templates\. That way, when developers select a service template, they have options for which environment to deploy their service to\.

Before creating a service from a service template or before publishing a service template, confirm that environments are deployed from the listed compatible environment templates\.

You *can't* update a service to the new major version if it's deployed to an environment that was built from a removed compatible environment template\.

To add or remove compatible environment templates for a service template version, you create a new major version of it\.

You can use the console or the AWS CLI to register and publish a service template\.

------
#### [ AWS Management Console ]

**Use the console to register and publish a new service template\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Service templates**\.

1. Choose **Create service template**\.

1. In the **Create service template** page, in the **Template bundle source** section, choose one of the available template options\.
   + **Use one of our sample template bundles**\.
   + **Use your own template bundle**\.
   + **Sync templates from Git**\.

1. 

**Provide a path to a template bundle\.**

   1. If you chose **Use one of our sample template bundles**:

      In the **Sample template bundle** section, select a sample template bundle\.

   1. If you chose **Sync templates from Git**, in the **Source code repository** section:

      1. Select the repository for your template sync configuration\.

      1. Enter the name of the repository branch to sync from\.

      1. \(Optional\) Enter name of a directory to limit the search for your template bundle\.

   1. Otherwise, in the **S3 bundle location** section, provide a path to your template bundle\.

1. In the **Template details** section\.

   1. Enter a **Template name**\.

   1. \(Optional\) Enter a **Template display name**\.

   1. \(Optional\) Enter a **Template description** for the service template\.

1. In the **Compatible environment templates** section, choose from a list of compatible environment templates\.

   

1. \(Optional\) In the **Encryption settings** section, choose **Customize encryption settings \(advanced\)** to provide your own encryption key\.

1. \(Optional\) In the **Pipeline** section:

   If you aren't including a service pipeline definition in your service template, uncheck the **Pipeline \- optional** check box at the bottom of the page\. You *can't* change this after the service template is created\. For more information, see [Template bundles](ag-template-authoring.md#ag-template-bundles)\.

1. \(Optional\) In the **Supported component sources** section, for **Component sources**, choose **Directly defined** to enable attachment of directly defined components to your service instances\.

1. \(Optional\) In the **Tags** section, choose **Add new tag** and enter a key and value to create a customer managed tag\.

1. Choose **Create a service template**\.

   You're now on a new page that displays the status and details for your new service template\. These details include a list of AWS and customer managed tags\. AWS Proton automatically generates AWS managed tags for you when you create AWS Proton resources\. For more information, see [AWS Proton resources and tagging](resources.md)\.

1. The status of a new service template status starts in the **Draft** state\. You and others with `proton:CreateService` permissions can view and access it\. Follow the next step to make the template available to others\.

1. In the **Template versions** section, choose the radio button to the left of the minor version of the template you just created \(1\.0\)\. As an alternative, you can choose **Publish** in the info alert and skip the next step\.

1. In the **Template versions** section, choose **Publish**\.

1. The template status changes to **Published**\. Because it's the latest version of the template, it's the **Recommended** version\.

1. In the navigation pane, select **Service templates** to view a list of your service templates and details\.

**Use the console to register new major and minor versions of a service template\.**

For more information, see [Versioned templates](ag-template-versions.md)\.

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Service Templates**\.

1. In the list of service templates, choose the name of the service template that you want to create a major or minor version for\.

1. In the service template detail view, choose **Create new version** in the **Template versions** section\.

1. In the **Create a new service template version** page, in the **Sample template bundle source** section, choose one of the two available template options\.
   + **Use one of our sample template bundles**\.
   + **Use your own template bundle**\.

1. Provide a path to the selected template bundle\.
   + If you chose **Use one of our sample template bundles**, in the **Sample template bundle** section, select a sample template bundle\.
   + If you chose **Use your own template bundle**, in the **S3 bundle location** section, choose the path to your template bundle\.

1. In the **Template details** section\.

   1. \(Optional\) Enter a **Template display name**\.

   1. \(Optional\) Enter a **Template description** for the service template\.

1. In the **Template details** section, choose one of the following options\.
   + To create a minor version, keep the check box **Check to create a new major version** empty\.
   + To create a major version, check the check box **Check to create a new major version**\.

1. Continue through the console steps to create the new minor or major version and choose **Create new version**\.

------
#### [ AWS CLI ]

To create service template that deploys a service without a service pipeline, add the parameter and value `--pipeline-provisioning "CUSTOMER_MANAGED"` to the `create-service-template` command\. Configure your template bundles as described in [Template bundles](ag-template-authoring.md#ag-template-bundles) creation and [Schema requirements for service template bundles](ag-schema.md#schema-req-svc)\.

**Note**  
You can't modify `pipelineProvisioning` after the service template is created\.

1. 

**Use the CLI to register and publish a new service template, with or without a service pipeline, as shown in the following steps\.**

   1. 

**Create a service template with a service pipeline using the CLI\.**

      Specify the name, display name \(optional\), and description \(optional\)\.

      Run the following command:

      ```
      $ aws proton create-service-template \
          --name "fargate-service" \
          --display-name "Fargate" \
          --description "Fargate-based Service"
      ```

      Response:

      ```
      {
          "serviceTemplate": {
              "arn": "arn:aws:proton:region-id:123456789012:service-template/fargate-service",
              "createdAt": "2020-11-11T23:02:55.551000+00:00",
              "description": "Fargate-based Service",
              "displayName": "Fargate",
              "lastModifiedAt": "2020-11-11T23:02:55.551000+00:00",
              "name": "fargate-service"
          }
      }
      ```

   1. 

**Create a service template without a service pipeline\.**

      Add `--pipeline-provisioning`\.

      Run the folllowing command:

      ```
      $ aws proton create-service-template \
          --name "fargate-service" \
          --display-name "Fargate" \
          --description "Fargate-based Service" \
          --pipeline-provisioning "CUSTOMER_MANAGED"
      ```

      Response:

      ```
      {
          "serviceTemplate": {
              "arn": "arn:aws:proton:region-id:123456789012:service-template/fargate-service",
              "createdAt": "2020-11-11T23:02:55.551000+00:00",
              "description": "Fargate-based Service",
              "displayName": "Fargate",
              "lastModifiedAt": "2020-11-11T23:02:55.551000+00:00",
              "name": "fargate-service",
              "pipelineProvisioning": "CUSTOMER_MANAGED"
          }
      }
      ```

1. 

**Create a minor version 0 of major version 1 of the service template\.**

   Include the template name, compatible environment templates, major version, and the S3 bucket name and key for the bucket that contains your service template bundle\.

   Run the following command:

   ```
   $ aws proton create-service-template-version \
       --template-name "fargate-service" \
       --description "Version 1" \
       --source s3="{bucket=your_s3_bucket, key=your_s3_key}" \
       --compatible-environment-templates '[{"templateName":"simple-env","majorVersion":"1"}]'
   ```

   Response:

   ```
   {
       "serviceTemplateMinorVersion": {
           "arn": "arn:aws:proton:region-id:123456789012:service-template/fargate-service:1.0",
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
           "status": "REGISTRATION_IN_PROGRESS",
           "templateName": "fargate-service"
       }
   }
   ```

   

1. Use the *get* command to check the registrations status\.

   Run the folllowing command:

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
           "arn": "arn:aws:proton:us-east-1:123456789012:service-template/fargate-service:1.0",
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

1. Publish the service template by using the update command to change the status to `"PUBLISHED"`\.

   Run the following command:

   ```
   $ aws proton update-service-template-version \
       --template-name "fargate-service" \
       --description "Version 1" \
       --major-version "1" \
       --minor-version "0" \
       --status "PUBLISHED"
   ```

   Response:

   ```
   {
       "serviceTemplateVersion": {
           "arn": "arn:aws:proton:region-id:123456789012:service-template/fargate-service:1.0",
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
           "recommendedMinorVersion": "0",
           "schema": "schema:\n  format:\n    openapi: \"3.0.0\"\n  pipeline_input_type: \"MyPipelineInputType\"\n  service_input_type: \"MyServiceInstanceInputType\"\n\n  types:\n    MyPipelineInputType:\n      type: object\n      description: \"Pipeline input properties\"\n      required:\n        - my_sample_pipeline_required_input\n      properties:\n        my_sample_pipeline_optional_input:\n          type: string\n          description: \"This is a sample input\"\n          default: \"hello pipeline\"\n        my_sample_pipeline_required_input:\n          type: string\n          description: \"Another sample input\"\n\n    MyServiceInstanceInputType:\n      type: object\n      description: \"Service instance input properties\"\n      required:\n        - my_sample_service_instance_required_input\n      properties:\n        my_sample_service_instance_optional_input:\n          type: string\n          description: \"This is a sample input\"\n          default: \"hello world\"\n        my_sample_service_instance_required_input:\n          type: string\n          description: \"Another sample input\"\n",
           "status": "PUBLISHED",
           "statusMessage": "",
           "templateName": "fargate-service"
       }
   }
   ```

1. Check that AWS Proton has published version 1\.0 by using the get command to retrieve service template detail data\.

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
           "arn": "arn:aws:proton:us-east-1:123456789012:service-template/fargate-service:1.0",
           "compatibleEnvironmentTemplates": [
               {
                   "majorVersion": "1",
                   "templateName": "simple-env"
               }
           ],
           "createdAt": "2020-11-11T23:02:57.912000+00:00",
           "description": "Version 1",
           "lastModifiedAt": "2020-11-11T23:03:04.767000+00:00",
           "majorVersion": "1",
           "minorVersion": "0",
           "schema": "schema:\n  format:\n    openapi: \"3.0.0\"\n  pipeline_input_type: \"MyPipelineInputType\"\n  service_input_type: \"MyServiceInstanceInputType\"\n\n  types:\n    MyPipelineInputType:\n      type: object\n      description: \"Pipeline input properties\"\n      required:\n        - my_sample_pipeline_required_input\n      properties:\n        my_sample_pipeline_optional_input:\n          type: string\n          description: \"This is a sample input\"\n          default: \"hello world\"\n        my_sample_pipeline_required_input:\n          type: string\n          description: \"Another sample input\"\n\n    MyServiceInstanceInputType:\n      type: object\n      description: \"Service instance input properties\"\n      required:\n        - my_sample_service_instance_required_input\n      properties:\n        my_sample_service_instance_optional_input:\n          type: string\n          description: \"This is a sample input\"\n          default: \"hello world\"\n        my_sample_service_instance_required_input:\n          type: string\n          description: \"Another sample input\"",
           "status": "PUBLISHED",
           "statusMessage": "",
           "templateName": "fargate-service"
       }
   }
   ```

------