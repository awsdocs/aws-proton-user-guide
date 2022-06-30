# Setting up with IAM<a name="ag-setting-up-iam"></a>

When you sign up for AWS, your AWS account is automatically signed up for all services in AWS, including AWS Proton\. You're charged only for the services and resources that you use\.

**Note**  
You and your team, including administrators and developers, must all be under the same account\.

## Sign up for AWS<a name="setting-up-aws-sign-up"></a>

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

## Create an IAM user<a name="setting-up-create-iam-user"></a>

**To create an administrator user for yourself and add the user to an administrators group \(console\)**

1. Sign in to the [IAM console](https://console.aws.amazon.com/iam/) as the account owner by choosing **Root user** and entering your AWS account email address\. On the next page, enter your password\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user that follows and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane, choose **Users** and then choose **Add users**\.

1. For **User name**, enter **Administrator**\.

1. Select the check box next to **AWS Management Console access**\. Then select **Custom password**, and then enter your new password in the text box\.

1. \(Optional\) By default, AWS requires the new user to create a new password when first signing in\. You can clear the check box next to **User must create a new password at next sign\-in** to allow the new user to reset their password after they sign in\.

1. Choose **Next: Permissions**\.

1. Under **Set permissions**, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, for **Group name** enter **Administrators**\.

1. Choose **Filter policies**, and then select **AWS managed \- job function** to filter the table contents\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.
**Note**  
You must activate IAM user and role access to Billing before you can use the `AdministratorAccess` permissions to access the AWS Billing and Cost Management console\. To do this, follow the instructions in [step 1 of the tutorial about delegating access to the billing console](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_billing.html)\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add metadata to the user by attaching tags as key\-value pairs\. For more information about using tags in IAM, see [Tagging IAM entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users and to give your users access to your AWS account resources\. To learn about using policies that restrict user permissions to specific AWS resources, see [Access management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.

## Setting up AWS Proton service roles<a name="setting-up-cicd"></a>

There are a few IAM roles that you might want to create for different parts of your AWS Proton solution\. You can create them in advance using the IAM console, or you can use the AWS Proton console to create them for you\.

Create an AWS Proton *environment service role* to allow AWS Proton to make API calls to other AWS services, like CloudFormation, on your behalf to provision resources for you using [AWS\-managed provisioning](ag-works-prov-methods.md#ag-works-prov-methods-direct)\. To learn more about the AWS Proton service role, see [AWS Proton service role](security_iam_service-role-policy-examples.md#proton-svc-role)\. When you [create an environment](ag-create-env.md), you can use the AWS Proton console to choose an existing role or to create a role with administrative privileges for you\.

Create an AWS Proton *pipeline service role* to allow AWS Proton to make API calls to other services, like CloudFormation, on your behalf to provision a CI/CD pipeline for you using [AWS\-managed provisioning](ag-works-prov-methods.md#ag-works-prov-methods-direct)\. To learn more about the AWS Proton service pipeline role, see [AWS Proton pipeline service role](security_iam_service-role-policy-examples.md#codepipeline-proton-svc-role)\. For more information about configuring CI/CD settings, see [Setting up account CI/CD settings](setting-up-for-service.md#setting-up-pr-pipelines)\.

**Note**  
Because we don't know which resources you will define in your AWS Proton templates, the role that you create using the console has broad permissions and can be used to act as both the AWS Proton pipeline service role and the AWS Proton service role\. For production deployments, we recommend that you scope down the permissions to the specific resources that will be deployed by creating customized policies for both the AWS Proton pipeline service role and the AWS Proton service role\. You can create and customize these roles by using the AWS CLI or IAM\. For more information, see [Service roles for AWS Proton](security_iam_service-with-iam.md#security_iam_service-with-iam-roles-service) and [Create a service](ag-create-svc.md)\.