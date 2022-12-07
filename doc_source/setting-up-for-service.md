# Setting up with AWS Proton<a name="setting-up-for-service"></a>

If you want to use the AWS CLI to run AWS Proton APIs, verify that you have installed it\. If you haven’t installed it, see [Setting up the AWS CLI](#ag-setting-up-cli)\.

**AWS Proton specific configuration:**
+ **To create and manage templates:**
  + If you're using [template sync configurations](ag-template-sync-configs.md), set up an [AWS CodeStar connection](#setting-up-vcontrol)\.
  + Otherwise, set up an [Amazon S3 bucket\.](#setting-up-bucket)
+ **To provision infrastructure:**
  + For [self\-managed provisioning](ag-works-prov-methods.md#ag-works-prov-methods-self), you must set up an [AWS CodeStar connection](#setting-up-vcontrol)\.
+ **\(Optional\) To provision pipelines:**
  + For [AWS\-managed provisioning](ag-works-prov-methods.md#ag-works-prov-methods-direct) and [CodeBuild\-based provisioning](ag-works-prov-methods.md#ag-works-prov-methods-codebuild), set up [pipeline roles](#setting-up-pr-role)\.
  + For [self\-managed provisioning](ag-works-prov-methods.md#ag-works-prov-methods-self), set up a [pipeline repository](#setting-up-pr-repo)\. 

For more information about provisioning methods, see [How AWS\-managed provisioning works](ag-works-prov-methods.md#ag-works-prov-methods-direct)\.

## Setting up an Amazon S3 bucket<a name="setting-up-bucket"></a>

To set up an S3 bucket, follow the instructions at [Create your first S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-bucket.html) to set up an S3 bucket\. Place your inputs to AWS Proton in the bucket where AWS Proton can retrieve them\. These inputs are known as template bundles\. You can learn more about them in other sections of this guide\.

## Setting up an AWS CodeStar connection<a name="setting-up-vcontrol"></a>

To connect AWS Proton to a repository, you create an AWS CodeStar connection that activates a pipeline when a new commit is made on a third\-party source code repository\.

**AWS Proton uses the connection to:**
+ Activate a service pipeline when a new commit is made on your repository source code\.
+ Make a pull request on an infrastructure as code repository\.
+ Create a new template minor or major version whenever a commit is pushed to a template repository that changes one of your templates, if the version doesn’t already exist\.

You can connect to Bitbucket, GitHub, GitHub Enterprise and GitHub Enterprise Server repositories with AWS CodeStar connections\. For more information, see [AWS CodeStar connections](https://docs.aws.amazon.com/codepipeline/latest/userguide/action-reference-CodestarConnectionSource.html) in the *AWS CodePipeline User Guide*\.

**To set up a CodeStar connection\.**

1. Open the [AWS Proton console](https://console.aws.amazon.com/proton/)\.

1. In the navigation pane, select **Settings** and then **Repository connections** to take you to the **Connections** page in **Developer Tools** **Settings**\. The page displays a list of connections\.

1. Choose **Create connection** and follow the instructions\.

## Setting up account CI/CD pipeline settings<a name="setting-up-pr-pipelines"></a>

AWS Proton can provision CI/CD pipelines for deploying application code into your service instances\. The AWS Proton settings you need for pipeline provisioning depend on the provisioning method you choose for your pipeline\.

### AWS\-managed and CodeBuild\-based provisioning—set up pipeline roles<a name="setting-up-pr-role"></a>

With [AWS\-managed provisioning](ag-works-prov-methods.md#ag-works-prov-methods-direct) and [CodeBuild provisioning](ag-works-prov-methods.md#ag-works-prov-methods-codebuild), AWS Proton provisions pipelines for you\. Therefore, AWS Proton needs a service role that provides permissions for provisioning pipelines\. Each one of these two provisioning methods uses its own service role\. These roles are shared across all AWS Proton service pipelines and you configure them once in your account settings\.

**To create pipeline service roles using the console**

1. Open the [AWS Proton console](https://console.aws.amazon.com/proton/)\.

1. In the navigation pane, choose **Settings**, and then choose **Account settings**\.

1. In the **Account CI/CD settings** page, choose **Configure**\.

1. Do one of the following:
   + **To have AWS Proton create a pipeline service role for you**

     \[To enable AWS\-managed provisioning of pipelines\] In the **Configure account settings** page, in the **AWS\-managed provisioning pipeline role** section:

     1. Select **New service role**\.

     1. Enter a name for the role, for example, **myProtonPipelineServiceRole**\.

     1. Check the check box to agree to create an AWS Proton role with administrative privileges in your account\.

     \[To enable CodeBuild\-based provisioning of pipelines\] In the **Configure account settings** page, in the **CodeBuild pipeline role** section, choose **Existing service role**, and choose the service role that you created in the **CloudFormation pipeline role** section\. Or, if you did not assign a CloudFormation pipeline role, repeat the previous three steps to create a new service role\.
   + **To choose existing pipeline service roles**

     \[To enable AWS\-managed provisioning of pipelines\] In the **Configure account settings** page, in the **AWS\-managed provisioning pipeline role** section, choose **Existing service role**, and choose a service role in your AWS account\.

     \[To enable CodeBuild provisioning of pipelines\] In the **Configure account settings** page, in the **CodeBuild pipeline provisioning role** section, choose **Existing service role**, and choose a service role in your AWS account\.

1. Choose **Save changes**\.

   Your new pipeline service role is displayed on the **Account settings** page\.

### Self\-managed provisioning—set up a pipeline repository<a name="setting-up-pr-repo"></a>

With [self\-managed provisioning](ag-works-prov-methods.md#ag-works-prov-methods-self), AWS Proton sends a pull request \(PR\) to a provisioning repository that you have set up, and your automation code is responsible for provisioning pipelines\. Therefore, AWS Proton doesn't need a service role to provision pipelines\. Instead, it needs a registered provisioning repository\. Your automation code in the repository has to assume an appropriate role that provides permissions for provisioning pipelines\.

**To register a pipeline provisioning repository using the console**

1. Create a CI/CD pipeline provisioning repository if you haven't yet created one\. For more information about pipelines in self\-managed provisioning, see [How self\-managed provisioning works](ag-works-prov-methods.md#ag-works-prov-methods-self)\.

1. In the navigation pane, choose **Settings**, and then choose **Account settings**\.

1. In the **Account CI/CD settings** page, choose **Configure**\.

1. In the **Configure account settings** page, in the **CI/CD pipeline repository** section:

   1. Select **New repository**, and then choose one of the repository providers\.

   1. For **CodeStar connection**, choose one of your connections\.
**Note**  
If you don't yet have a connection to the relevant repository provider account, choose **Add a new CodeStar connection**, complete the connection creation process, and then choose the refresh button next to the **CodeStar connection** menu\. You should now be able to choose your new connection in the menu\.

   1. For **Repository name**, choose your pipeline provisioning repository\. The drop\-down menu shows the list of repositories in the provider account\.

   1. For **Branch name**, choose one of the repository branches\.

1. Choose **Save changes**\.

   Your pipeline repository is displayed on the **Account settings** page\.

## Setting up the AWS CLI<a name="ag-setting-up-cli"></a>

To use the AWS CLI to make AWS Proton API calls, verify that you have installed the latest version of the AWS CLI\. For more information, see [Getting started with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\. Then, to get started using the AWS CLI with AWS Proton, see [Getting started with the AWS CLI](ag-getting-started-cli.md)\.