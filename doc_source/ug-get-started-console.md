# Getting started with the AWS Management Console<a name="ug-get-started-console"></a>

**Get started with AWS Proton**
+ Create and view a service\.
+ Deploy a service\.
+ Delete a service\.

## Step 1: Set up an example source repository<a name="getting-started-step1"></a>

**Set up repository\.**

In *Getting started*, you create a service from a service template that includes a service pipeline\. You provide a path to your forked sample source code repository and your repository connection Amazon Resource Name \(ARN\) as inputs when you create your service\. Your repository connection signals AWS Proton on new source code commits\. 
+ Fork the example [AWS sample repository](https://github.com/aws-samples/aws-proton-sample-fargate-service) that you use as source code to deploy with AWS Proton in *Getting started*\.

## Step 2: Create a service<a name="getting-started-step2"></a>

1. Open the [AWS Proton console](https://console.aws.amazon.com/proton/)\.

1. In the navigation pane, choose **Services**\.

1. In the **Services** page, choose **Create service**\.

1. In the **Choose a service template** page, select the **My Fargate Service** template by choosing the radio button at the top\-right corner of the template card\.

   If you don't see this template listed, contact your administrator or follow the [Setting up](https://docs.aws.amazon.com/proton/latest/adminguide/ag-setting-up.html) and [Getting started](https://docs.aws.amazon.com/proton/latest/adminguide/ag-getting-started-console.html) procedures in the *AWS Proton Administrator Guide*\.

1. Choose **Configure** at the lower right corner of the page\.

1. In the **Configure service** page, in the **Service settings** section, enter the service name **my\-service**\.

1. \(Optional\) Enter a description for the service\.

1. In the **Service repository settings** section, enter the name for the **Branch** and **Repository ID** \(repo\-username/repo\-name\) where you forked the [source code](proton-setup.md#setup-repo-connection)\.

1. For **Repository Connection**, choose your repository connection from the list\. Then, either proceed to the next step, or choose **Next**\.

1. \(Optional\) In the **Tags** section, choose **Add new tag** and enter a key and value to create a customer managed tag\. Then choose **Next**\.

1. In the **Configure custom settings** page, in the **Service instances** section, in the **New instance** section, follow the next steps to provide custom values for your service instance parameters\.

   1. Enter the instance name **my\-app\-service**\.

   1. Choose the environment **my\-fargate\-environment** for your service instance\.

   1. Keep the defaults for the remaining instance parameters\.

   1. Keep the defaults for **Pipeline inputs**\.

   1. Choose **Next** and review your inputs\.

   1. Choose **Create** and view your service status and details\.

1. In the service details page, view the status of your service instance and pipeline by choosing the **Overview** and **Pipeline** tabs\. On these pages you can also view AWS and customer managed tags\. AWS Proton automatically creates AWS managed tags for you\. Choose **Manage tags** to create and modify customer managed tags\. For more information about tagging, see [AWS Proton resources and tagging](resources.md)\.

1. After the service is **Active**, in the **Overview** tab, in the **Service Instances** section, choose the name of your service instance, **my\-app\-service**\.

   You are now on the service instance detail page\.

1. To view your application, in the **Outputs** section, copy the **ServiceEndpoint** link to your browser\.

   You see an AWS Proton graphic in the web page\.

1. After the service is created, in the navigation pane, choose **Services** to view a list of your services\.

## Step 3: Edit a service<a name="getting-started-step3"></a>

1. When your service status is **Active**, make a small update to your repository source code by changing 'AWS Proton' to 'AWS PROTON' for the graphic in `index.html`\. Commit the update to prompt your pipeline to make the update to your service instance\.

   The pipeline pulls the update\. The update is included in the next deployment of your service instance\.

1. Return to the [AWS Proton console](https://console.aws.amazon.com/proton/)\.

1. In the **Services** page, choose the service name **my\-service**\.

   You're now on the service detail page for **my\-service**\.

1. Choose the **Pipelines** tab\.

1. In the **Outputs** section, choose the **PipelineEndpoint** link\.

   This takes you to your service pipeline page\.

1. Choose **Release** to force the update\.

1. Navigate back to your service in the AWS Proton console to view the status of your service instance and application as the update is deployed\.

## Step 4: Delete a service<a name="getting-started-step4"></a>

1. In the navigation pane, choose **Services**\.

1. In the **Services** page, choose the service name **my\-service**\.

   You're now on the service detail page for **my\-service**\.

1. In the upper right\-hand corner of the page, choose **Actions** and then **Delete**\.

1. A modal prompts you to confirm the delete action\.

1. Follow the instructions and choose **Yes, delete**\.