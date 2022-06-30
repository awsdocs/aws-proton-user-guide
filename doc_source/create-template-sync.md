# Create a template sync configuration<a name="create-template-sync"></a>

Learn how to create a template sync configuration with AWS Proton\.

**Create a template sync configuration prerequisites:**
+ A [registered a repository](ag-create-repo.md) with AWS Proton\.
+ A [template bundle](ag-template-authoring.md#ag-template-bundles) is located in your repository\.

**The repository registration consists of the following:**
+ An AWS CodeStar connections connection that gives AWS Proton permission to access your repository and subscribe to its notifications\.
+ A [service linked role](using-service-linked-roles.md)\. When you register your repository, the service linked role is created for you\.

Before you create your first template sync configuration, push a template bundle to your repository as shown in the following directory layout\.

```
 /templates/                                                 # subdirectory (optional)
 /templates/my-env-template/                                 # template name
 /templates/my-env-template/v1/                              # template version
 /templates/my-env-template/v1/infrastructure/               # template bundle
 /templates/my-env-template/v1/schema/
```

After you create your first template sync configuration, new template versions are automatically created when you push a commit that adds an updated template bundle under a new version \(for example, under `/my-env-template/v2/`\)\.

```
 /templates/                                                 # subdirectory (optional)
 /templates/my-env-template/                                 # template name
 /templates/my-env-template/v1/                              # template version
 /templates/my-env-template/v1/infrastructure/               # template bundle
 /templates/my-env-template/v1/schema/
 /templates/my-env-template/v2/
 /templates/my-env-template/v2/infrastructure/
 /templates/my-env-template/v2/schema/
```

You can include new template bundle versions for one or more sync configured templates in a single commit\. AWS Proton creates a new template version for each new template bundle version that was included in the commit\.

After you created the template sync configuration, you can still manually create new versions of the template in the console or with the AWS CLI by uploading template bundles from an S3 bucket\. Template syncing only works in one direction: from your repository to AWS Proton\. Manually created template versions *aren’t* synced\.

After you set up a template sync configuration, AWS Proton listens for changes to your repository\. Whenever a change is pushed, it looks for any directory that has the same name as your template\. It then looks inside that directory for any directories that look like major versions\. AWS Proton registers the template bundle to the corresponding template major version\. The new versions are always in the `DRAFT` state\. You can [publish the new versions](template-create.md) with the console or AWS CLI\.

For example, suppose you have a template that's called `my-env-template` configured to sync from `my-repo/templates` on branch `main` with the following layout\.

```
 /code
 /code/service.go
 README.md
 /templates/
 /templates/my-env-template/
 /templates/my-env-template/v1/
 /templates/my-env-template/v1/infrastructure/
 /templates/my-env-template/v1/schema/
 /templates/my-env-template/v2/
 /templates/my-env-template/v2/infrastructure/
 /templates/my-env-template/v2/schema/
```

AWS Proton syncs the contents of `/templates/my-env-template/v1/` to `my-env-template:1` and the contents of `/templates/my-env-template/v2/` to `my-env-template:2`\. If they don’t already exist, it creates these major versions\.

AWS Proton found the first directory that matched the template name\. You can limit the directories AWS Proton searches by specifying a `subdirectoryPath` when you create or edit a template sync configuration\. For example, you can specify `/production-templates/` for `subdirectoryPath`\.

You can create a template sync configuration using the console or CLI\.

------
#### [ AWS Management Console ]

**Create a template and template sync configuration using the console\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environment templates**\.

1. Choose **Create environment template**\.

1. In the **Create environment template** page, in the **Template options** section, choose **Create a template for provisioning new environments**\.

1. In the **Template bundle source** section, choose **Sync templates from Git**\.

1. In the **Source code repository** section:

   1. For **Repository**, select the repository that contains your template bundle\.

   1. For **Branch**, select a repository branch to sync from\.

   1. \(Optional\) For **Template bundle directory**, enter the name of a directory to scope down the search for your template bundle\.

1. In the **Template details** section\.

   1. Enter a **Template name**\.

   1. \(Optional\) Enter a **Template display name**\.

   1. \(Optional\) Enter a **Template description** for the environment template\.

1. \(Optional\) Check the checkbox for **Customize encryption settings \(advanced\)** in the **Encryption settings** section to provide your own encryption key\.

1. \(Optional\) In the **Tags** section, choose **Add new tag** and enter a key and value to create a customer managed tag\.

1. Choose **Create Environment template**\.

   You're now on a new page that displays the status and details for your new environment template\. These details include a list of AWS managed and customer managed tags\. AWS Proton automatically generates AWS managed tags for you when you create AWS Proton resources\. For more information, see [AWS Proton resources and tagging](resources.md)\.

1. In the template detail page, choose the **Sync** tab to view template sync configuration detail data\.

1. Choose the **Template versions** tab to view template versions with status details\.

1. The status of a new environment template status starts in the **Draft** state\. You and others with `proton:CreateEnvironment` permissions can view and access it\. Follow the next step to make the template available to others\.

1. In the **Template versions** section, choose the radio button to the left of the minor version of the template that you just created \(1\.0\)\. As an alternative, you can choose **Publish** in the info alert and skip the next step\.

1. In the **Template versions** section, choose **Publish**\.

1. The template status changes to **Published**\. It's the latest and **Recommended** version of the template\.

1. In the navigation pane, select **Environment templates** to view a list of your environment templates and details\.

The procedure for creating a service template and template sync configuration is similar\.

------
#### [ AWS CLI ]

**Create a template and template sync configuration using the AWS CLI\.**

1. 

**Create a template\. In this example, an environment template is created\.**

   Run the following command\.

   ```
   $ aws proton create-environment-template \
       --name "env-template"
   ```

   The response is as follows\.

   ```
   {
       "environmentTemplate": {
           "arn": "arn:aws:proton:us-east-2:123456789012:environment-template/env-template",
           "createdAt": "2021-11-07T23:32:43.045000+00:00",
           "displayName": "env-template",
           "lastModifiedAt": "2021-11-07T23:32:43.045000+00:00",
           "name": "env-template",
           "status": "DRAFT",
           "templateName": "env-template"
       }
   }
   ```

1. 

**Create your template sync configuration with AWS CLI by providing the following:**
   + The template that you want to sync to\. After you have created the template sync configuration, you can still create new versions from it manually in the console or with the AWS CLI\.
   + The template name\.
   + The template type\.
   + The repository that you want to sync from\.
   + The repository provider type\.
   + The branch where the template bundle is located\.
   + \(Optional\) The path to the directory containing your template bundle\. By default, AWS Proton looks for the first directory that matches your template name\.

   Run the following command\.

   ```
   $ aws proton create-template-sync-config \
       --template-name "env-template" \
       --template-type "ENVIRONMENT" \
       --repository-name "myrepos/templates" \
       --repository-provider "GITHUB" \
       --branch "main" \
       --subdirectory "env-template/"
   ```

   The response is as follows\.

   ```
   {
       "templateSyncConfigDetails": {
           "branch": "main",
           "repositoryName": "myrepos/templates",
           "repositoryProvider": "GITHUB",
           "subdirectory": "templates",
           "templateName": "env-template",
           "templateType": "ENVIRONMENT"
       }
   }
   ```

1. **To publish your template version, see [Register and publish templates](template-create.md)\.**

------

## Syncing service templates<a name="create-template-sync-service-templates"></a>

The preceding examples show how you can sync environment templates\. Service templates are similar\. To sync service templates you add an additional file named `.template-registration.yaml` to each major version directory in your template bundle\. This file contains additional details that AWS Proton needs when it creates a service template version for you following a commit\. When you explicitly create a service template version using the AWS Proton console or API, you provide these details as inputs, and this file replaces these inputs for template sync\.

```
./templates/                                                 # subdirectory (optional)
 /templates/my-svc-template/                                 # service template name
 /templates/my-svc-template/v1/                              # service template version
 /templates/my-svc-template/v1/.template-registration.yaml   # service template version properties
 /templates/my-svc-template/v1/infrastructure/               # template bundle
 /templates/my-svc-template/v1/schema/
```

The `.template-registration.yaml` file contains the following details:
+ **Compatible environments** \[required\] – Environments based on these environment templates and major versions are compatible with services based on this service template version\.
+ **Supported component sources** \[optional\] – Components using these sources are compatible with services based on this service template version\. If not specified, components can't be attached to these services\. For more information about components, see [AWS Proton components](ag-components.md)\.

The file's YAML syntax is as follows:

```
compatible_environments:
  - env-templ-name:major-version
  - ...
supported_component_sources:
  - DIRECTLY_DEFINED
```

Specify one or more environment template / major version combinations\. Specifying `supported_component_sources` is optional, and the only supported value is `DIRECTLY_DEFINED`\.

**Example \.template\-registration\.yaml**  
In this example, the service template version is compatible with major versions 1 and 2 of the `my-env-template` environment template\. It's also compatible with the major versions 1 and 3 of the `another-env-template` environment template\. The file doesn't specify `supported_component_sources`, so components can't be attached to services based on this service template version\.  

```
compatible_environments:
  - my-env-template:1
  - my-env-template:2
  - another-env-template:1
  - another-env-template:3
```

**Note**  
Previously, AWS Proton defined a different file, `.compatible-envs`, for specifying compatible environments\. AWS Proton still supports that file and its format for backward compatibility\. We don't recommend using it anymore, because it isn't extensible and can't support newer features like components\.