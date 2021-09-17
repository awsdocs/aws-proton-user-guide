# Setting up with AWS Proton<a name="proton-setup"></a>

You need to set up a repository connection to create services with service templates that include a service pipeline\.

Verify that you have installed the AWS CLI if you plan to use it to make AWS Proton API calls\.

## Setting up a repository connection<a name="setup-repo-connection"></a>

To create a service, you start by selecting a service template that defines your service infrastructure\. In the console, you provide inputs to the template as you create a service from a service template\. If you select a service template that includes a standard AWS Proton service pipeline, you must provide your code repository name and branch, along with a repository connection ARN\. AWS Proton uses the repository connection to signal the service and pipeline in response to your source code commits\.

This is one of the [Prerequisites](getting-started-prerequisites.md) for following the steps in [Getting started with the AWS Management Console](ug-get-started-console.md)\.

**Set up a repository connection\.**

Connect AWS Proton to your code repository with AWS CodeStar connections to prompt an AWS Proton service pipeline when a new push is made to your repository\. You can connect to Bitbucket, GitHub, GitHub Enterprise and GitHub Enterprise Server repositories with AWS CodeStar connections\. For more information, see [AWS CodeStar connections](https://docs.aws.amazon.com/codepipeline/latest/userguide/action-reference-CodestarConnectionSource.html) in the * AWS CodePipeline User Guide*\.

1. Open the [AWS Proton console](https://console.aws.amazon.com/proton/)\.

1. In the navigation pane, select **Settings** and then **Source connections** to take you to the **Connections** page of **Developer Tools** **Settings**\. The page displays a list of repository connections\.

1. Choose **Create connection** and follow the instructions\.

## Setting up with the AWS CLI<a name="setting-up-cli"></a>

To use the AWS CLI to make AWS Proton API calls, verify that you have installed the latest version of the AWS CLI\. For more information, see [https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)