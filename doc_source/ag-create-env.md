# Create an environment<a name="ag-create-env"></a>

Learn to create AWS Proton environments\.

**You can create two types of environments:**
+ Create, manage, and provision a standard environment by using a *standard environment template*\. AWS Proton provisions infrastructure for your environment\.
+ Connect AWS Proton to customer\-managed infrastructure by using a *customer\-managed environment template*\. You provision your own shared resources outside of AWS Proton, and then you provide provisioning outputs that AWS Proton can use\.

**You can choose one of several provisioning approaches when you create an environment\.**
+ *AWS\-managed provisioning* – Create, manage, and provision an environment in a single account\. AWS Proton provisions your environment\.

  This method only supports CloudFormation infrastructure as code \(IaC\) templates\.
+ *AWS\-managed provisioning to another account* – In a single management account, create and manage an environment that is provisioned in another account with environment account connections\. AWS Proton provisions your environment in the other account\. For more information, see [Create an environment in one account and provision in another account](#ag-create-env-deploy-other) and [Environment account connections](ag-env-account-connections.md)\.

  This method only supports CloudFormation IaC templates\.
+ *Self\-managed provisioning* – AWS Proton submits provisioning pull requests to a registered repository with your own provisioning infrastructure\.

  This method only supports Terraform IaC templates\.

With AWS\-managed provisioning \(both in the same account and to another account\), AWS Proton makes direct calls to provision your resources\.

With self\-managed provisioning, AWS Proton makes pull requests to provide compiled IaC files that your IaC engine uses to provision resources\.

For more information, see [How AWS Proton provisions infrastructure](ag-works-prov-methods.md), [Template bundles](ag-template-authoring.md#ag-template-bundles), and [Schema requirements for environment template bundles](ag-schema.md#schema-req-env)\.

**Topics**
+ [Create and provision a standard environment in the same account](#ag-create-env-same-account)
+ [Create an environment in one account and provision in another account](#ag-create-env-deploy-other)
+ [Create and provision an environment using self\-managed provisioning](#ag-create-env-pull-request)

## Create and provision a standard environment in the same account<a name="ag-create-env-same-account"></a>

Use the console or AWS CLI to create and provision an environment in a single account\. Provisioning is managed by AWS\.

------
#### [ AWS Management Console ]

**Use the console to create and provision an environment in a single account**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environments**\.

1. Choose **Create environment**\.

1. In the **Choose an environment template** page, select a template and choose **Configure**\.

1. In the **Configure environment** page, in the **Provisioning** section, choose **AWS\-managed provisioning**\.

1. In the **Deployment account** section, choose **This AWS account**\.

1. In the **Configure environment** page, in the **Environment settings** section, enter an **Environment name**\.

1. \(Optional\) Enter a description for the environment\.

1. In the **Environment roles** section, select the AWS Proton service role that you created as part of [Setting up AWS Proton service roles](ag-setting-up-iam.md#setting-up-cicd)\.

1. \(Optional—self\-managed provisioning\) In the **Component role** section, select a service role that enables directly defined components to run in the environment and scopes down the resources that they can provision\. For more information, see [AWS Proton components](ag-components.md)\.

1. \(Optional\) In the **Tags** section, choose **Add new tag** and enter a key and value to create a customer managed tag\.

1. Choose **Next**\.

1. In the **Configure environment custom settings** page, you must enter values for the `required` parameters\. You can enter values for the `optional` parameters or use the defaults when given\.

1. Choose **Next** and review your inputs\.

1. Choose **Create**\.

   View the environment details and status, as well as the AWS managed tags and customer managed tags for your environment\.

1. In the navigation pane, choose **Environments**\.

   A new page displays a list of your environments along with the status and other environment details\.

------
#### [ AWS CLI ]

**Use the AWS CLI to create and provision an environment in a single account\.**

To create an environment, you specify the [AWS Proton service role](security_iam_service-role-policy-examples.md#proton-svc-role) ARN, path to your spec file, environment name, environment template ARN, the major and minor versions, and description \(optional\)\.

The next examples shows a YAML formatted spec file that specifies values for two inputs, defined in the environment template schema file\. You can use the `get-environment-template-minor-version` command to view the environment template schema\.

Spec:

```
proton: EnvironmentSpec
spec:
  my_sample_input: "the first"
  my_other_sample_input: "the second"
```

Create an environment\.

Run the following command:

```
$ aws proton create-environment \
    --name "MySimpleEnv" \
    --template-name simple-env \
    --template-major-version 1 \
    --proton-service-role-arn "arn:aws:iam::123456789012:role/AWSProtonServiceRole" \
    --spec "file://env-spec.yaml"
```

Response:

```
{
    "environment": {
        "arn": "arn:aws:proton:region-id:123456789012:environment/MySimpleEnv",
        "createdAt": "2020-11-11T23:03:05.405000+00:00",
        "deploymentStatus": "IN_PROGRESS",
        "lastDeploymentAttemptedAt": "2020-11-11T23:03:05.405000+00:00",
        "name": "MySimpleEnv",
        "protonServiceRoleArn": "arn:aws:iam::123456789012:role/ProtonServiceRole",
        "templateName": "simple-env"
    }
}
```

After you create a new environment, you can view a list of AWS and customer managed tags, as shown in the following example command\. AWS Proton automatically generates AWS managed tags for you\. You can also modify and create customer managed tags using the AWS CLI\. For more information, see [AWS Proton resources and tagging](resources.md)\.

Command:

```
$ aws proton list-tags-for-resource \
    --resource-arn "arn:aws:proton:region-id:123456789012:environment/MySimpleEnv"
```

------

## Create an environment in one account and provision in another account<a name="ag-create-env-deploy-other"></a>

Use the console or AWS CLI to create a standard environment in a management account that provisions environment infrastructure in another account\. Provisioning is managed by AWS\.

**Before using the console or CLI, complete the following steps\.**

1. Identify the AWS account IDs for the management and environment account, and copy them for later use\.

1. In the environment account, create an AWS Proton service role with minimum permissions for the environment to create\. For more information, see [AWS Proton service role](security_iam_service-role-policy-examples.md#proton-svc-role)\.

------
#### [ AWS Management Console ]

**Use the console create an environment in one account and provision in another\.**

1. 

**In the environment account, create an environment account connection, and use it to send a request to connect to the management account\.**

   1. In [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environment account connections** in the navigation pane\.

   1. In the **Environment account connections** page, choose **Request to connect**\.
**Note**  
Verify that the account ID listed in the **Environment account connection** page heading matches your pre\-identified environment account ID\.

   1. In the **Request to connect** page, in the **Environment role** section, select **Existing service role** and the name of the service role that you created for the environment\.

   1. In the **Connect to management account** section, enter the **Management account ID** and an **Environment name** for your AWS Proton environment\. Copy the name for later use\.

   1. Choose **Request to connect** at the lower right corner of the page\.

   1. Your request shows as pending in the **Environment connections sent to a management account** table and a modal shows how to accept the request from the management account\.

1. 

**In the management account, accept a request to connect from the environment account\.**

   1. Log in to your management account and choose **Environment account connections** in the AWS Proton console\.

   1. In the **Environment account connections** page, in the **Environment account connection requests** table, select the environment account connection with the environment account ID that matches your pre\-identified environment account ID\.
**Note**  
Verify that the account ID listed in the **Environment account connection** page heading matches your pre\-identified management account ID\.

   1. Choose **Accept**\. The status changes from pending to connected\.

1. 

**In the management account, create an environment\.**

   1. Choose **Environment templates** in the navigation pane\.

   1. In the **Environment templates** page, choose **Create environment template**\.

   1. In the **Choose an environment template** page, choose an environment template\.

   1. In the **Configure environment** page, in the **Provisioning** section, choose **AWS\-managed provisioning**\.

   1. In the **Deployment account** section, choose **Another AWS account**\.

   1. In the **Environment details** section, select your **Environment account connection** and **Environment name**\.

   1. Choose **Next**\.

   1. Fill out the forms and choose **Next** until you reach the **Review and Create** page\.

   1. Review and choose **Create environment**\.

------
#### [ AWS CLI ]

**Use the AWS CLI to create an environment in one account and provision in another\.**

In the environment account, create an environment account connection and request to connect\.

Run the following command:

```
$ aws proton create-environment-account-connection \
    --environment-name "simple-env-connected" \
    --role-arn "arn:aws:iam::123456789222:role/service-role/env-account-proton-service-role" \
    --management-account-id "123456789111"
```

Response:

```
{
    "environmentAccountConnection": {
        "arn": "arn:aws:proton:region-id:123456789222:environment-account-connection/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "environmentAccountId": "123456789222",
        "environmentName": "simple-env-connected",
        "id": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "lastModifiedAt": "2021-04-28T23:13:50.847000+00:00",
        "managementAccountId": "123456789111",
        "requestedAt": "2021-04-28T23:13:50.847000+00:00",
        "roleArn": "arn:aws:iam::123456789222:role/service-role/env-account-proton-service-role",
        "status": "PENDING"
    }
}
```

In the management account, accept the environment account connection request\.

Run the following command:

```
$ aws proton accept-environment-account-connection \
    --id "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111"
```

Response:

```
{
    "environmentAccountConnection": {
        "arn": "arn:aws:proton:region-id:123456789222:environment-account-connection/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "environmentAccountId": "123456789222",
        "environmentName": "simple-env-connected",
        "id": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "lastModifiedAt": "2021-04-28T23:15:33.486000+00:00",
        "managementAccountId": "123456789111",
        "requestedAt": "2021-04-28T23:13:50.847000+00:00",
        "roleArn": "arn:aws:iam::123456789222:role/service-role/env-account-proton-service-role",
        "status": "CONNECTED"
    }
}
```

View your environment account connection by using get\.

Run the following command:

```
$ aws proton get-environment-account-connection \
    --id "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111"
```

Response:

```
{
    "environmentAccountConnection": {
        "arn": "arn:aws:proton:region-id:123456789222:environment-account-connection/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "environmentAccountId": "123456789222",
        "environmentName": "simple-env-connected",
        "id": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "lastModifiedAt": "2021-04-28T23:15:33.486000+00:00",
        "managementAccountId": "123456789111",
        "requestedAt": "2021-04-28T23:13:50.847000+00:00",
        "roleArn": "arn:aws:iam::123456789222:role/service-role/env-account-proton-service-role",
        "status": "CONNECTED"
    }
}
```

In the management account, create an environment\.

Run the following command:

```
$ aws proton create-environment \
    --name "simple-env-connected" \
    --template-name simple-env-template \
    --template-major-version "1" \
    --template-minor-version "1" \
    --spec "file://simple-env-template/specs/original.yaml" \
    --environment-account-connection-id "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111"
```

Response:

```
{
    "environment": {
        "arn": "arn:aws:proton:region-id:123456789111:environment/simple-env-connected",
        "createdAt": "2021-04-28T23:02:57.944000+00:00",
        "deploymentStatus": "IN_PROGRESS",
        "environmentAccountConnectionId": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "environmentAccountId": "123456789222",
        "lastDeploymentAttemptedAt": "2021-04-28T23:02:57.944000+00:00",
        "name": "simple-env-connected",
        "templateName": "simple-env-template"
    }
}
```

------

## Create and provision an environment using self\-managed provisioning<a name="ag-create-env-pull-request"></a>

When you use self\-managed provisioning, AWS Proton submits provisioning pull requests to a registered repository with your own provisioning infrastructure\. The pull requests trigger your own workflow, which calls AWS services to provision infrastructure\.

**Self\-managed provisioning considerations:**
+ Before you start the following environment creation procedure, set up a repository resource directory for self\-managed provisioning\. For more information, see [AWS Proton infrastructure as code files](ag-infrastructure-tmp-files.md)\.
+ After you create the environment, AWS Proton waits to receive asynchronous notifications regarding the status of your infrastructure provisioning\. Your provisioning code must use the AWS Proton `NotifyResourceStateChange` API to send these asynchronous notifications to AWS Proton\.

You can use self\-managed provisioning in the console or with the AWS CLI\. The following examples show how you can use self\-managed provisioning with Terraform\.

------
#### [ AWS Management Console ]

**Use the console to create a Terraform environment using self\-managed provisioning\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environments**\.

1. Choose **Create environment**\.

1. In the **Choose an environment template** page, select a Terraform template and choose **Configure**\.

1. In the **Configure environment** page, in the **Provisioning** section, choose **Self\-managed provisioning**\.

1. In the **Provisioning repository details** section:

   1. If you haven't yet [registered your provisioning repository with AWS Proton](ag-create-repo.md), choose **New repository**, choose one of the repository providers, and then, for **CodeStar connection**, choose one of your connections\.
**Note**  
If you don't yet have a connection to the relevant repository provider account, choose **Add a new CodeStar connection**, complete the connection creation process, and then choose the refresh button next to the **CodeStar connection** menu\. You should now be able to choose your new connection in the menu\.

      If you've already registered your repository with AWS Proton, choose **Existing repository**\.

   1. For **Repository name**, choose a repository\. The drop\-down menu shows registered repositories for **Existing repository** or the list of repositories in the provider account for **New repository**\.

   1. For **Branch name**, choose one of the repository branches\.

1. In the **Environment settings** section, enter an **Environment name**\.

1. \(Optional\) Enter a description for the environment\.

1. \(Optional\) In the **Tags** section, choose **Add new tag** and enter a key and value to create a customer managed tag\.

1. Choose **Next**\.

1. In the **Configure environment custom settings** page, you must enter values for the `required` parameters\. You can enter values for the `optional` parameters or use the defaults when given\.

1. Choose **Next** and review your inputs\.

1. Choose **Create** to send a pull request\.
   + If you approve the pull request, the deployment is in progress\.
   + If you reject the pull request, the environment creation is cancelled\.
   + If the pull request times out, environment creation *isn't* complete\.

1. View the environment details and status, as well as the AWS managed tags and customer managed tags for your environment\.

1. In the navigation pane, choose **Environments**\.

   A new page displays a list of your environments along with the status and other environment details\.

------
#### [ AWS CLI ]

When you create an environment using self\-managed provisioning, you *add* the `provisioningRepository` parameter and omit the `ProtonServiceRoleArn` and `environmentAccountConnectionId` parameters\.

**Use the AWS CLI to create a Terraform environment with self\-managed provisioning\.**

1. Create an environment and send a pull request to the repository for review and approval\.

   The next examples shows a YAML formatted spec file that defines values for two inputs, based on the environment template schema file\. You can use the `get-environment-template-minor-version` command to view the environment template schema\.

   Spec:

   ```
   proton: EnvironmentSpec
   spec:
     ssm_parameter_value: "test"
   ```

   Create an environment:

   Command:

   ```
   $ aws proton create-environment \
       --name "pr-environment" \
       --template-name "pr-env-template" \
       --template-major-version "1" \
       --provisioning-repository="branch=main,name=myrepos/env-repo,provider=GITHUB" \
       --spec "file://env-spec.yaml"
   ```

   Response:

   ```
   {
       "environment": {
           "arn": "arn:aws:proton:region-id:123456789012:environment/pr-environment",
           "createdAt": "2021-11-18T17:06:58.679000+00:00",
           "deploymentStatus": "IN_PROGRESS",
           "lastDeploymentAttemptedAt": "2021-11-18T17:06:58.679000+00:00",
           "name": "pr-environment",
           "provisioningRepository": {
               "arn": "arn:aws:proton:region-id:123456789012:repository/github:myrepos/env-repo",
               "branch": "main",
               "name": "myrepos/env-repo",
               "provider": "GITHUB"
           },
           "templateName": "pr-env-template"
       }
   ```

1. Review the request\.
   + If you approve the request, provisioning is in progress\.
   + If you reject the request, the environment creation is cancelled\.
   + If the pull request times out, environment creation *isn't* complete\.

1. Asynchronously provide provisioning status to AWS Proton\. The following example notifies AWS Proton of a successful provisioning\.

   ```
   $ aws proton notify-resource-deployment-status-change \
       --resource-arn "arn:aws:proton:region-id:123456789012:environment/pr-environment" \
       --status "SUCCEEDED"
   ```

------