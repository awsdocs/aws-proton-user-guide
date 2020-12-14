--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# Setting up<a name="getting-started-setting-up"></a>

## Setting up with IAM<a name="getting-setup-iam"></a>

**To sign up for AWS and create an administrator account**

1. If you don't have an AWS account, open [https://aws\.amazon\.com/](https://aws.amazon.com/), choose **Create an AWS Account**, and then complete the following steps to create one\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Groups**, and then choose **Create New Group**\.

1. For **Group Name**, type a name for your group \(for example **Administrators**\), and then choose **Next Step**\.

1. In the list of policies, select the check box next to the **AdministratorAccess** policy\. You can use the **Filter** menu and the **Search** box to filter the list of policies\.

1. Choose **Next Step**, choose **Create Group**, and then your new group appears under **Group Name**\.

1. In the navigation pane, choose **Users**, and then choose **Create New Users**\.

1. In box **1**, type a user name, clear the check box next to **Generate an access key for each user**, and then choose **Create**\.

1. In the list of users, choose the name \(not the check box\) of the user you just created\. You can use the **Search** box to search for the user name\.

1. Choose the **Groups** tab and then choose **Add User to Groups**\.

1. Select the check box next to the administrators group and then choose **Add to Groups**\.

1. Choose the **Security Credentials** tab, and then, under **Sign\-In Credentials**, choose **Manage Password**\.

1. Select **Assign a custom password**, type a password in the **Password** and **Confirm Password** boxes, and then choose **Apply**\.

## Install AWS CLI Proton APIs<a name="setting-up-cli"></a>

Make Proton API calls using the AWS CLI by completing the following steps\.

1. 

**Prerequisites**

   You have installed the latest version of the AWS CLI\. For more information, see [https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)

1. 

**Install the Proton APIs**

   Run the following commands to enable Proton APIs for the AWS CLI\. 

   ```
   aws s3 cp s3://aws-proton-preview-public-files/model/proton-2020-07-20.normal.json .
   ```

   ```
   aws s3 cp s3://aws-proton-preview-public-files/model/waiters2.json .
   ```

   ```
   aws configure add-model --service-model "file://proton-2020-07-20.normal.json" --service-name proton-preview
   ```

   ```
   mv waiters2.json ~/.aws/models/proton-preview/2020-07-20/waiters-2.json
   ```

   ```
   rm proton-2020-07-20.normal.json
   ```