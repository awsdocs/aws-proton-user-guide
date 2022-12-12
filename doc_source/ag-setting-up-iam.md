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

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)\.

## Create an IAM user<a name="setting-up-create-iam-user"></a>

To create an administrator user, choose one of the following options\.


****  

| Choose one way to manage your administrator | To | By | You can also | 
| --- | --- | --- | --- | 
| In IAM Identity Center \(Recommended\) | Use short\-term credentials to access AWS\.This aligns with the security best practices\. For information about best practices, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-users-federation-idp) in the *IAM User Guide*\. | Following the instructions in [Getting started](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html) in the AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide\. | Configure programmatic access by [Configuring the AWS CLI to use AWS IAM Identity Center \(successor to AWS Single Sign\-On\)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html) in the AWS Command Line Interface User Guide\. | 
| In IAM \(Not recommended\) | Use long\-term credentials to access AWS\. | Following the instructions in [Creating your first IAM admin user and user group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the IAM User Guide\. | Configure programmatic access by [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the IAM User Guide\. | 

## Setting up AWS Proton service roles<a name="setting-up-cicd"></a>

There are a few IAM roles that you might want to create for different parts of your AWS Proton solution\. You can create them in advance using the IAM console, or you can use the AWS Proton console to create them for you\.

Create AWS Proton *environment roles* to allow AWS Proton to make API calls to other AWS services, like AWS CloudFormation, AWS CodeBuild, and various compute and storage services, on your behalf to provision resources for you\. A *AWS\-managed provisioning role* is required when an environment or any of the service instances running in it use [AWS\-managed provisioning](ag-works-prov-methods.md#ag-works-prov-methods-direct)\. A *CodeBuild role* is required when an environment or any of its service instances use [CodeBuild provisioning](ag-works-prov-methods.md#ag-works-prov-methods-codebuild)\. To learn more about the AWS Proton environment roles, see [IAM Roles](ag-environment-roles.md)\. When you [create an environment](ag-create-env.md), you can use the AWS Proton console to choose an existing role for either of these two roles, or to create a role with administrative privileges for you\.

Similarly, create AWS Proton *pipeline roles* to allow AWS Proton to make API calls to other services on your behalf to provision a CI/CD pipeline for you\. To learn more about the AWS Proton pipeline roles, see [AWS Proton pipeline service roles](security_iam_service-role-policy-examples.md#codepipeline-proton-svc-role)\. For more information about configuring CI/CD settings, see [Setting up account CI/CD pipeline settings](setting-up-for-service.md#setting-up-pr-pipelines)\.

**Note**  
Because we don't know which resources you will define in your AWS Proton templates, the roles that you create using the console have broad permissions and can be used as both the AWS Proton pipeline service roles and the AWS Proton service roles\. For production deployments, we recommend that you scope down the permissions to the specific resources that will be deployed by creating customized policies for both the AWS Proton pipeline service roles and the AWS Proton environment service roles\. You can create and customize these roles by using the AWS CLI or IAM\. For more information, see [Service roles for AWS Proton](security_iam_service-with-iam.md#security_iam_service-with-iam-roles-service) and [Create a service](ag-create-svc.md)\.