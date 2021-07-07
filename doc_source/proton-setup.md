# Setting up with AWS Proton<a name="proton-setup"></a>

You need to set up a repository connection to be able to select service templates that include a service pipeline\. Verify that you have installed the AWS CLI if you plan to run AWS Proton commands using it\.

## Setting up a repository connection<a name="setup-repo-connection"></a>

**Set up repository\.**
+ Fork the example [AWS sample repository](https://github.com/aws-samples/aws-proton-sample-fargate-service) that you can use as source code to deploy with AWS Proton\.

**Set up a repository connection\.**

Connect AWS Proton to your code repository with AWS CodeStar connections to prompt an AWS Proton service pipeline when a new push is made to your repository\. You can connect to Bitbucket, GitHub, GitHub Enterprise and GitHub Enterprise Server repositories with AWS CodeStar connections\. For more information, see [AWS CodeStar connections](https://docs.aws.amazon.com/codepipeline/latest/userguide/action-reference-CodestarConnectionSource.html) in the * AWS CodePipeline User Guide*\.

1. Open the [AWS Proton console](https://console.aws.amazon.com/proton/)\.

1. In the navigation pane, select **Settings** and then **Source connections** to take you to the **Connections** page of **Developer Tools** **Settings**\. The page displays a list of repository connections\.

1. Choose **Create connection** and follow the instructions\.

## Setting up with the AWS CLI<a name="setting-up-cli"></a>

To use the AWS CLI to make AWS Proton API calls, verify that you have installed the latest version of the AWS CLI\. For more information, see [https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)