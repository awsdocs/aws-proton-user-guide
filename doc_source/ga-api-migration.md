# AWS Proton GA API Migration<a name="ga-api-migration"></a>

AWS Proton was launched in December 2020 at re:invent for public preview\. We were really excited to introduce AWS Proton to customers\. It’s been 6 months since then\. During that time we’ve been developing the features that are important to customers and making sure AWS Proton lives up to the operational standards of AWS\. Additionally, we’ve been refining our API\.

Unless you’re using the console, an API is the way you communicate with us\. We want to make that communication as easy and natural as possible\. After meeting with our customers and carefully re\-examining our APIs, we decided that the API needed some changes\.

Unfortunately many of these are breaking changes\. Now, the AWS SDK *doesn’t* break APIs\. So we purposely didn’t launch in the AWS SDK for our public preview release because we wanted to keep API improvement opportunities open\. That is why you could only access AWS Proton through the console and the AWS CLI \(using [this](https://docs.aws.amazon.com/cli/latest/reference/configure/add-model.html) neat feature\)\.

Soon we’ll be launching in GA and we’ll be added to the AWS SDK\. Between May 20 and our GA release, we’ll be rolling out the new API changes\. The console will continue to work\. If you use the CLI, either in your terminal or in your service pipeline, at some point you will likely encounter breakages\. We apologize for the inconvenience\. We trust you’ll find that the new API improvements are well worth the migration\. For more information on migrating your AWS Proton resources, continue on to the following sections\.

## Migrate the CLI to the New API<a name="migrate-cli"></a>

If your CLI commands are using the old API, you’ll get notified via exception\. In fact, that exception might have linked you here\.

First, you’ll want to update the Proton model in your CLI:

```
$ aws s3 cp s3://aws-proton-preview-public-files/model/proton-2020-07-20.normal.json .
$ aws s3 cp s3://aws-proton-preview-public-files/model/waiters2.json .
$ aws configure add-model --service-model file://proton-2020-07-20.normal.json --service-name proton-preview
$ mv waiters2.json ~/.aws/models/proton-preview/2020-07-20/waiters-2.json
$ rm proton-2020-07-20.normal.json
```

Then you’ll need to start using the new parameters and APIs\. Generally, the CLI does a good job of letting you know which parameters are now required\. For example, the command:

```
$ aws proton-preview create-environment-template --template-name "my-template"
```

becomes:

```
$ aws proton-preview create-environment-template --name "my-template"
```

For a complete list of the changes, see the next section\.

## Itemized API changes<a name="api-changes"></a>


**Exception renames**  

| Current name | New name | 
| --- | --- | 
|  InvalidEncryptionKeyException  |  ValidationException  | 
|  InvalidInputException  |  ValidationException  | 
|  ResourceAlreadyExistsException  |  ConflictException  | 
|  ConflictingOperationException  |  ConflictException  | 
|  LimitExceededException  |  ServiceQuotaExceededException  | 
|  ThrottledException  |  ThrottlingException  | 
|  NotFoundException  |  ResourceNotFoundException  | 
|  ServiceException  |  InternalServerException  | 


**Input parameter renames**  

| API | Current name | New name | Required | Behavior change | 
| --- | --- | --- | --- | --- | 
|  CreateEnvironment  |  environmentName environmentTemplateArn templateMajorVersionId templateMinorVersionId  |  name templateName templateMajorVersion templateMinorVersion  |  Yes Yes Yes No  |  \- ARN to String \- \-  | 
|  UpdateEnvironment  |  environmentName templateMajorVersionId templateMinorVersionId versionUpdateType  |  name templateMajorVersion templateMinorVersion deploymentType  |  Yes No No Yes  |  \- \- \- \-  | 
|  DeleteEnvironment  |  environmentName  |  name  |  Yes  |  \-  | 
|  GetEnvironment  |  environmentName  |  name  |  Yes  |  \-  | 
|  ListEnvironments  |  environmentTemplateMajorVersionArns  |  environmentTemplates  |  No  |  Data type only  | 
|  CreateEnvironmentTemplate  |  templateName  |  name  |  Yes  |  \-  | 
|  UpdateEnvironmentTemplate  |  templateName  |  name  |  Yes  |  \-  | 
|  DeleteEnvironmentTemplate  |  templateName  |  name  |  Yes  |  \-  | 
|  GetEnvironmentTemplate  |  templateName  |  name  |  Yes  |  \-  | 
|  UpdateEnvironmentTemplateMajorVersion  |  majorVersionId  |  majorVersion  |  Yes  |  \-  | 
|  DeleteEnvironmentTemplateMajorVersion  |  majorVersionId  |  majorVersion  |  Yes  |  \-  | 
|  GetEnvironmentTemplateMajorVersion  |  majorVersionId  |  majorVersion  |  Yes  |  \-  | 
|  CreateEnvironmentTemplateMinorVersion  |  majorVersionId  |  majorVersion  |  Yes  |  \-  | 
|  UpdateEnvironmentTemplateMinorVersion  |  majorVersionId minorVersionId  |  majorVersion minorVersion  |  Yes Yes  |  \- \-  | 
|  DeleteEnvironmentTemplateMinorVersion  |  majorVersionId minorVersionId  |  majorVersion minorVersion  |  Yes Yes  |  \- \-  | 
|  GetEnvironmentTemplateMinorVersion  |  majorVersionId minorVersionId  |  majorVersion minorVersion  |  Yes Yes  |  \- \-  | 
|  ListEnvironmentTemplateMinorVersions  |  majorVersionId  |  majorVersion  |  No  |  \-  | 
|  CreateService  |  serviceName serviceTemplateArn templateMajorVersionId templateMinorVersionId  |  name templateName templateMajorVersion templateMinorVersion  |  Yes Yes Yes No  |  \- ARN to String \- \-  | 
|  UpdateService  |  serviceName  |  name  |  Yes  |  \-  | 
|  DeleteService  |  serviceName  |  name  |  Yes  |  \-  | 
|  GetService  |  serviceName  |  name  |  Yes  |  \-  | 
|  GetServiceInstance  |  serviceInstanceName  |  name  |  Yes  |  \-  | 
|  UpdateServiceInstance  |  serviceInstanceName templateMajorVersionId templateMinorVersionId versionUpdateType  |  name templateMajorVersion templateMinorVersion deploymentType  |  Yes No No Yes  |  \- \- \- \-  | 
|  UpdateServicePipeline  |  templateMajorVersionId templateMinorVersionId versionUpdateType  |  templateMajorVersion templateMinorVersion deploymentType  |  No No Yes  |  \- \- \-  | 
|  CreateServiceTemplate  |  templateName pipelineEnabled  |  name pipelineProvisioning  |  Yes No  |  \- Boolean to Provisioning  | 
|  UpdateServiceTemplate  |  templateName  |  name  |  Yes  |  \-  | 
|  DeleteServiceTemplate  |  templateName  |  name  |  Yes  |  \-  | 
|  GetServiceTemplate  |  templateName  |  name  |  Yes  |  \-  | 
|  UpdateServiceTemplateMajorVersion  |  majorVersionId  |  majorVersion  |  Yes  |  \-  | 
|  DeleteServiceTemplateMajorVersion  |  majorVersionId  |  majorVersion  |  Yes  |  \-  | 
|  GetServiceTemplateMajorVersion  |  majorVersionId  |  majorVersion  |  Yes  |  \-  | 
|  CreateServiceTemplateMinorVersion  |  majorVersionId  |  majorVersion  |  Yes  |  \-  | 
|  UpdateServiceTemplateMinorVersion  |  majorVersionId minorVersionId  |  majorVersion minorVersion  |  Yes Yes  |  \- \-  | 
|  DeleteServiceTemplateMinorVersion  |  majorVersionId minorVersionId  |  majorVersion minorVersion  |  Yes Yes  |  \- \-  | 
|  GetServiceTemplateMinorVersion  |  majorVersionId minorVersionId  |  majorVersion minorVersion  |  Yes Yes  |  \- \-  | 
|  ListServiceTemplateMinorVersions  |  majorVersionId  |  majorVersion  |  No  |  \-  | 


**Output parameter renames**  

| Resource | Current name | New name | Behavior change | 
| --- | --- | --- | --- | 
|  Environment  |  environmentName lastDeploymentAttemptTime lastSuccessfulDeploymentTime environmentTemplateArn templateMajorVersionId templateMinorVersionId  |  name lastDeploymentAttemptedAt lastDeploymentSucceededAt templateName templateMajorVersion templateMinorVersion  |  \- \- \- ARN to String \- \-  | 
|  EnvironmentTemplate  |  templateName  |  name  |  \-  | 
|  EnvironmentTemplateMajorVersion  |  majorVersionId  |  majorVersion  |  \-  | 
|  EnvironmentTemplateMinorVersion  |  majorVersionId minorVersionId  |  majorVersion minorVersion  |  \- \-  | 
|  Service  |  serviceName serviceTemplateArn  |  name templateName  |  \- ARN to String  | 
|  ServiceInstance  |  serviceInstanceName lastDeploymentAttemptTime lastSuccessfulDeploymentTime environmentArn serviceTemplateArn templateMajorVersionId templateMinorVersionId  |  name lastDeploymentAttemptedAt lastDeploymentSucceededAt environmentName templateName templateMajorVersion templateMinorVersion  |  \- \- \- ARN to String ARN to String \- \-  | 
|  ServicePipeline  |  lastDeploymentAttemptTime lastSuccessfulDeploymentTime serviceTemplateArn templateMajorVersionId templateMinorVersionId  |  lastDeploymentAttemptedAt lastDeploymentSucceededAt templateName templateMajorVersion templateMinorVersion  |  \- \- ARN to String \- \-  | 
|  ServiceTemplate  |  templateName  |  name  |  \-  | 
|  ServiceTemplateData  |  pipelineEnabled  |  pipelineProvisioning  |  \-  | 
|  ServiceTemplateMajorVersion  |  majorVersionId  |  majorVersion  |  \-  | 
|  ServiceTemplateMinorVersion  |  majorVersionId minorVersionId  |  majorVersion minorVersion  |  \- \-  | 


**Enum renames**  

| Enum type | Current value | New value | 
| --- | --- | --- | 
|  ServiceStatus  |  CREATE\_STARTED UPDATE\_STARTED DELETE\_STARTED  |  CREATE\_IN\_PROGRESS UPDATE\_IN\_PROGRESS DELETE\_IN\_PROGRESS  | 
|  VersionUpdateType  |  NO\_DEPLOY UPDATE\_SPEC  |  NONE CURRENT\_VERSION  | 

**Other**

`ProvisionedStacks` and **Environment**, **Instance** and **Pipeline** `Outputs` properties have been removed\.

## Upgrade your templates to the new format<a name="template-format"></a>

Based on our learnings over the past 6 months, we have made some changes to the service and environment template file structure, as well as changes to the data presented to Jinja\. After AWS Proton goes GA, AWS Proton can't accept templates that use the old format and you'll see an error\. In fact, this type of error possibly brought you here\.

**Convert an environment template**

1. Rename the `engine` property in your `manifest.yaml` file as shown in the following examples\.

   Before:

   ```
   infrastructure:
       templates:
           - file: "cloudformation.yaml"
           engine: jinja
           template_language: cloudformation
   ```

   After:

   ```
   infrastructure:
       templates:
           - file: "cloudformation.yaml"
           rendering_engine: jinja
           template_language: cloudformation
   ```

1. Update your template Jinja name\-spaces as shown in the following example\.

   Change:

   ```
   environment.my_param
   ```

   To:

   ```
   environment.inputs.my_param
   ```

**Convert a service template**

1. Rename the `engine` property in your `manifest.yaml` file as shown in the following examples\.

   Before:

   ```
   infrastructure:
       templates:
           - file: "cloudformation.yaml"
           engine: jinja
           template_language: cloudformation
   ```

   After:

   ```
   infrastructure:
       templates:
           - file: "cloudformation.yaml"
           rendering_engine: jinja
           template_language: cloudformation
   ```

1. Rename the `infrastructure` directory to `instance_infrastructure`\.

1. Rename the `pipeline` directory to `pipeline_infrastructure`\.

1. Update your service instance template Jinja name\-spaces as shown in the following example\.

   1. Change:

      ```
      service_instance.my_param
      ```

      To:

      ```
      service_instance.inputs.my_param
      ```

   1. Change:

      ```
      service_name
      ```

      To:

      ```
      service.name
      ```

   1. Change:

      ```
      service_instance_name
      ```

      To:

      ```
      service_instance.name
      ```

1. Update your pipeline template Jinja name\-spaces as shown in the following example\.

   1. Change:

      ```
      service_name
      ```

      To:

      ```
      service.name
      ```

   1. Change:

      ```
      source.repository
      ```

      To:

      ```
      service.repository_id
      ```

   1. Change:

      ```
      source.branch
      ```

      To:

      ```
      service.branch_name
      ```

   1. Change:

      ```
      source.connection_arn
      ```

      To:

      ```
      service.repository_connection_arn
      ```

   1. Change:

      ```
      pipeline.my_param
      ```

      To:

      ```
      pipeline.inputs.my_param
      ```

   1. `service_instances` used to contain the inputs for each instance\. We realized it is more appropriate to provide the outputs as shown in the following examples\.

      Change:

      ```
      service_instances[0].spec.my_input
      ```

      To:

      ```
      service_instances[0].outputs.my_output
      ```

      `my_output` must be listed as an output in your instance infrastructure as code file\.