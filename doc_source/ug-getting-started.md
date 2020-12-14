--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# Getting started with AWS Proton<a name="ug-getting-started"></a>

**Get started with AWS Proton using the AWS Management Console**
+ Create and view service\.
+ Deploy a service\.
+ Delete a service\.

**Topics**
+ [Prerequisites](#getting-started-prerequisites)
+ [Setting up](getting-started-setting-up.md)
+ [Step 1: Create a GitHub repository and connection](#getting-started-step1)
+ [Step 2: Open the Proton console](#getting-started-step2)
+ [Step 3: Create service](#getting-started-step3)
+ [Step 4: Edit a service](#getting-started-step4)
+ [Step 5: Delete a service](#getting-started-step5)

## Prerequisites<a name="getting-started-prerequisites"></a>
+ An IAM account with administrator permissions\. For more information, see [Setting up](getting-started-setting-up.md) in the next section\.
**Note**  
For Public preview, administrators and environments must be under the same AWS account\.
+ A GitHub account\. For more information, see [GitHub](https://github.com/)\.
+ General familiarity with AWS and with using the AWS Management Console\.
+ You're logged into your AWS account\.

## Step 1: Create a GitHub repository and connection<a name="getting-started-step1"></a>

**Set up GitHub repository\.**
+ Fork the example [AWS sample repository](https://github.com/aws-samples/aws-proton-sample-fargate-service) that you can use as source code to deploy using Proton\.

**Set up a GitHub connection\.**

Set up this connection to trigger a Proton service pipeline when a new commit is made on the repository\.

1. Open the [Proton console](https://console.aws.amazon.com/proton/)\.

1. From the left\-hand menu, select **Settings** and then **Source connections** to take you to the **Connections** page of **Developer Tools** Code Suite under **Settings**\. The page displays a list of repository connections\.

1. Click on **Create connection** button and follow the instructions\.

## Step 2: Open the Proton console<a name="getting-started-step2"></a>

To start, you complete steps that are outlined in the Proton console's **Getting Started** section\.

1. Open the [Proton console](https://console.aws.amazon.com/proton/)\.

1. In the left\-hand menu, click on **Getting started**\.

## Step 3: Create service<a name="getting-started-step3"></a>

1. In **Getting started, Ready to launch: Developers deploy standardized services**, click on **Create and deploy a service**\.

1. From the **Choose a service template** page, select the **My Fargate Service** template by clicking on the radio button at the top\-right corner of the template\.

1. Click on the **Configure** button at the lower right corner of the page\.

   This takes you to the **Configure service** page\.

1. Enter the service name **my\-service**\.

1. \(Optional\) Enter a description for the service\.

1. In the **Service repository settings**, enter the name of the GitHub branch where you forked the source code\.

1. Enter your repository ID\.

1. For **Repository Connection**, choose your GitHub connection from the list and then click on **Next**\.

1. From the **Configure service custom settings** page, click on the **Add new instance** button, in the **New instance** section\.

   1. Enter the instance name **my\-app\-service**\.

   1. Choose the environment **my\-fargate\-environment** for the service instance\.

   1. Keep the defaults for the remaining **New instance** inputs\.

   1. Keep the defaults for **Pipeline inputs** inputs\.

   1. Click on the **Create**\.

1. On the service details page, view the status of your service instance and pipeline by clicking on the **Overview** and **Pipeline** tabs\.

1. After the service is created, in the left\-hand menu, click on **Services** to view a list of your services\.

## Step 4: Edit a service<a name="getting-started-step4"></a>

1. Make a small update to your GitHub repository source code that does not modify the application functionality\. Push the update to trigger your pipeline to make the update to your service\.

1. Return to the [Proton console](https://console.aws.amazon.com/proton/)\.

1. In the **Services** page, click on the service name **my\-service**\.

   You're now on the service detail page for **my\-service**\.

1. Click on the **Pipelines** tab\.

1. View the status of your triggered pipeline\.

## Step 5: Delete a service<a name="getting-started-step5"></a>

1. In the left\-hand menu, click on **Services**\.

1. In the **Services** page, click on the service name **my\-service**\.

   You're now on the service detail page for **my\-service**\.

1. In the upper right\-hand corner, click on **Actions** and then **Delete service**\.