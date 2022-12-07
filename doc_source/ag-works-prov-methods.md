# How AWS Proton provisions infrastructure<a name="ag-works-prov-methods"></a>

AWS Proton can provision infrastructure in one of several ways:
+ **AWS\-managed provisioning** – AWS Proton calls the provisioning engine on your behalf\. This method supports only AWS CloudFormation template bundles\. For more information, see [AWS CloudFormation IaC files](ag-infrastructure-tmp-files-cloudformation.md)\.
+ **CodeBuild provisioning** – AWS Proton uses AWS CodeBuild to run shell commands that you provide\. Your commands can read inputs that AWS Proton provides, and are responsible for provisioning or deprovisioning infrastructure and generating output values\. A template bundle for this method includes your commands in a manifest file and any programs, scripts, or other files that these commands may need\.

  As an example to using CodeBuild provisioning, you can include code that uses the AWS Cloud Development Kit \(AWS CDK\) to provision AWS resources, and a manifest that installs the CDK and runs your CDK code\.

  For more information, see [CodeBuild provisioning template bundle](ag-infrastructure-tmp-files-codebuild.md)\.
**Note**  
You can use CodeBuild provisioning with environments and services\. At this time you can't provision components this way\.
+ **self\-managed provisioning** – AWS Proton issues a pull request \(PR\) to a repository that you provide, where your own infrastructure deployment system runs the provisioning process\. This method supports only Terraform template bundles\. For more information, see [Terraform IaC files](ag-infrastructure-tmp-files-terraform.md)\.

AWS Proton determines and sets the provisioning method for each environment and service separately\. When you create or update an environment or a service, AWS Proton examines the template bundle that you provide, and determines the provisioning method that the template bundle indicates\. At the environment level, you provide the parameters that the environment and its potential services might need for their provisioning methods—AWS Identity and Access Management \(IAM\) roles, an environment account connection, or an infrastructure repository\.

Developers who use AWS Proton to provision a service have the same experience regardless of provisioning method\. Developers don't need to be aware of the provisioning method and don't need to change anything in the service provisioning process\. The service template sets the provisioning method, and each environment that a developer deploys the service to provides the necessary parameters for service instance provisioning\.

The following diagram summarizes some major traits of the different provisioning methods\. The sections that follow the table provide details about each method\.


| Provisioning method | Templates | Provisioned by | Status tracked by | 
| --- | --- | --- | --- | 
| AWS\-managed | manifest, schema, IaC file \(CloudFormation\) | AWS Proton \(through CloudFormation\) | AWS Proton \(through CloudFormation\) | 
| CodeBuild | manifest \(with commands\), schema, command dependencies \(e\.g\. AWS CDK code\) | AWS Proton \(through CodeBuild\) | AWS Proton \(your commands return status through CodeBuild\) | 
| self\-managed | manifest, schema, IaC files \(Terraform\) | Your code \(through Git actions\) | Your code \(passed to AWS through API call\) | 

## How AWS\-managed provisioning works<a name="ag-works-prov-methods-direct"></a>

When an environment or a service uses AWS\-managed provisioning, infrastructure is provisioned as follows:

1. An AWS Proton customer \(an administrator or a developer\) creates the AWS Proton resource \(an environment or a service\)\. The customer selects a template for the resource and provides the required parameters\. For more information, see the following section, [Considerations for AWS\-managed provisioning](#ag-works-prov-methods-direct-consider)\.

1. AWS Proton renders a complete AWS CloudFormation template for provisioning the resource\.

1. AWS Proton calls AWS CloudFormation to start provisioning using the rendered template\.

1. AWS Proton continuously monitors the AWS CloudFormation deployment\.

1. When provisioning completes, AWS Proton reports back errors in case of failure, and captures provisioning outputs, like Amazon VPC ID, in case of success\.

The following diagram shows that AWS Proton takes care of most of these steps directly\.

![\[Diagram illustrating AWS-managed provisioning in AWS Proton\]](http://docs.aws.amazon.com/proton/latest/userguide/images/provisioning-direct.png)

### Considerations for AWS\-managed provisioning<a name="ag-works-prov-methods-direct-consider"></a>
+ *Infrastructure provisioning role* – When an environment or any of the service instances running in it might use AWS\-managed provisioning, an administrator needs to configure an IAM role \(either directly or as part of an AWS Proton environment account connection\)\. AWS Proton uses this role to provision the infrastructure of these AWS\-managed provisioning resources\. The role should have permissions to use AWS CloudFormation to create all the resources that the templates of these resources include\.

  For more information, see [IAM Roles](ag-environment-roles.md) and [AWS Proton IAM service role policy examples](security_iam_service-role-policy-examples.md)\.
+ *Service provisioning* – When a developer deploys a service instance that uses AWS\-managed provisioning to the environment, AWS Proton uses the role provided to that environment to provision infrastructure for the service instance\. Developers don't see this role and can't change it\.
+ *Service with pipeline* – A service template that uses AWS\-managed provisioning may include a pipeline definition written in the AWS CloudFormation YAML schema\. AWS Proton also creates the pipeline by calling AWS CloudFormation\. The role that AWS Proton uses to create a pipeline is separate from the role for each individual environment\. This role is provided to AWS Proton separately, only once at the AWS account level, and it's used to provision and manage all AWS\-managed pipelines\. This role should have permissions to create pipelines and other resources that your pipelines need\.

  The following procedures show how to provide the pipeline role to AWS Proton\.

------
#### [ AWS Proton console ]

**To provide the pipeline role**

  1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), on the navigation pane, choose **Settings > Account settings**, and then choose **Configure**\.

  1. Use the **Pipeline AWS\-managed role** section to configure a new or existing pipeline role for AWS\-managed provisioning\.

------
#### [ AWS Proton API ]

**To provide the pipeline role**

  1. Use the [UpdateAccountSettings](https://docs.aws.amazon.com/proton/latest/APIReference/API_UpdateAccountSettings.html) API action\.

  1. Provide the Amazon Resource Name \(ARN\) of your pipeline service role in the `pipelineServiceRoleArn` parameter\.

------
#### [ AWS CLI ]

  **To provide the pipeline role**

  Run the following command:

  ```
  $ aws proton update-account-settings \
      --pipeline-service-role-arn \
          "arn:aws:iam::123456789012:role/my-pipeline-role"
  ```

------

## How CodeBuild provisioning works<a name="ag-works-prov-methods-codebuild"></a>

When an environment or a service uses CodeBuild provisioning, infrastructure is provisioned as follows:

1. An AWS Proton customer \(an administrator or a developer\) creates the AWS Proton resource \(an environment or a service\)\. The customer selects a template for the resource and provides the required parameters\. For more information, see the following section, [Considerations for CodeBuild provisioning](#ag-works-prov-methods-codebuild-consider)\.

1. AWS Proton renders an input file with input parameter values for provisioning the resource\.

1. AWS Proton calls CodeBuild to start a job\. The CodeBuild job runs the customer shell commands specified in the template\. These commands provision the desired infrastructure, while optionally reading input values\.

1. When provisioning completes, the final customer command returns the provisioning status to CodeBuild and calls the [NotifyResourceDeploymentStatusChange](https://docs.aws.amazon.com/proton/latest/APIReference/API_NotifyResourceDeploymentStatusChange.html) AWS Proton API action to provide outputs, like Amazon VPC ID, if any exist\.
**Important**  
Be sure that your commands correctly return the provisioning status to CodeBuild and provide the outputs\. If they don't, AWS Proton can't properly track the provisioning status and can't provide correct outputs to service instances\.

The following diagram illustrates the steps that AWS Proton performs and the steps that your commands perform within a CodeBuild job\.

![\[Diagram illustrating CodeBuild-based provisioning in AWS Proton\]](http://docs.aws.amazon.com/proton/latest/userguide/images/provisioning-codebuild.png)

### Considerations for CodeBuild provisioning<a name="ag-works-prov-methods-codebuild-consider"></a>
+ *Infrastructure provisioning role* – When an environment or any of the service instances running in it might use CodeBuild\-based provisioning, an administrator needs to configure an IAM role \(either directly or as part of an AWS Proton environment account connection\)\. AWS Proton uses this role to provision the infrastructure of these CodeBuild provisioning resources\. The role should have permissions to use CodeBuild to create all the resources that your commands in the templates of these resources provision\.

  For more information, see [IAM Roles](ag-environment-roles.md) and [AWS Proton IAM service role policy examples](security_iam_service-role-policy-examples.md)\.
+ *Service provisioning* – When a developer deploys a service instance that uses CodeBuild provisioning to the environment, AWS Proton uses the role provided to that environment to provision infrastructure for the service instance\. Developers don't see this role and can't change it\.
+ *Service with pipeline* – A service template that uses CodeBuild provisioning may include commands to provision a pipeline\. AWS Proton also creates the pipeline by calling CodeBuild\. The role that AWS Proton uses to create a pipeline is separate from the role for each individual environment\. This role is provided to AWS Proton separately, only once at the AWS account level, and it's used to provision and manage all CodeBuild\-based pipelines\. This role should have permissions to create pipelines and other resources that your pipelines need\.

  The following procedures show how to provide the pipeline role to AWS Proton\.

------
#### [ AWS Proton console ]

**To provide the pipeline role**

  1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), on the navigation pane, choose **Settings > Account settings**, and then choose **Configure**\.

  1. Use the **Codebuild pipeline provisioning role** section to configure a new or existing pipeline role for CodeBuild provisioning\.

------
#### [ AWS Proton API ]

**To provide the pipeline role**

  1. Use the [UpdateAccountSettings](https://docs.aws.amazon.com/proton/latest/APIReference/API_UpdateAccountSettings.html) API action\.

  1. Provide the Amazon Resource Name \(ARN\) of your pipeline service role in the `pipelineCodebuildRoleArn` parameter\.

------
#### [ AWS CLI ]

  **To provide the pipeline role**

  Run the following command:

  ```
  $ aws proton update-account-settings \
      --pipeline-codebuild-role-arn \
          "arn:aws:iam::123456789012:role/my-pipeline-role"
  ```

------

## How self\-managed provisioning works<a name="ag-works-prov-methods-self"></a>

When an environment is configured to use self\-managed provisioning, infrastructure is provisioned as follows:

1. An AWS Proton customer \(an administrator or a developer\) creates the AWS Proton resource \(an environment or a service\)\. The customer selects a template for the resource and provides the required parameters\. For an environment, the customer also provides a linked infrastructure repository\. For more information, see the following section, [Considerations for self\-managed provisioning](#ag-works-prov-methods-self-consider)\.

1. AWS Proton renders a complete Terraform template\. It consists of one or more Terraform files, potentially in multiple folders, and a `.tfvars` variables file\. AWS Proton writes parameter values provided on the resource creation call into this variables file\.

1. AWS Proton submits a PR to the infrastructure repository with the rendered Terraform template\.

1. When the customer \(administrator or developer\) merges the PR, the customer's automation triggers the provisioning engine to start provisioning infrastructure using the merged template\.
**Note**  
If the customer \(administrator or developer\) closes the PR, AWS Proton recognizes the PR as closed and marks the deployment as cancelled\.

1. When provisioning completes, the customer's automation calls the [NotifyResourceDeploymentStatusChange](https://docs.aws.amazon.com/proton/latest/APIReference/API_NotifyResourceDeploymentStatusChange.html) AWS Proton API action to indicate completion, provide the status \(success or failure\), and provide outputs, like Amazon VPC ID, if any exist\.
**Important**  
Be sure that your automation code calls back into AWS Proton with the provisioning status and outputs\. If it doesn't, AWS Proton might consider the provisioning as pending for longer than it should, and keep showing **In progress** status\.

The following diagram illustrates the steps that AWS Proton performs and the steps that your own provisioning system performs\.

![\[Diagram illustrating self-managed provisioning in AWS Proton\]](http://docs.aws.amazon.com/proton/latest/userguide/images/provisioning-self.png)

### Considerations for self\-managed provisioning<a name="ag-works-prov-methods-self-consider"></a>
+ *Infrastructure repository* – When an administrator configures an environment for self\-managed provisioning, they need to provide a linked infrastructure repository\. AWS Proton submits PRs to this repository to provision the infrastructure of the environment and all the service instances that are deployed to it\. The customer\-owned automation action in the repository should assume an IAM role with permissions to create all the resources that your environment and service templates include, and an identity that reflects the destination AWS account\. For an example GitHub Action that assumes a role, see [Assuming a Role](https://github.com/aws-actions/configure-aws-credentials#assuming-a-role) in the *"Configure AWS Credentials" Action For GitHub Actions* documentation\.
+ *Permissions* – Your provisioning code has to authenticate with an account as necessary \(for example, authenticate to an AWS account\) and provide resource provisioning authorization \(for example, provide a role\)\.
+ *Service provisioning* – When a developer deploys a service instance that uses self\-managed provisioning to the environment, AWS Proton submits a PR to the repository that is associated with the environment to provision infrastructure for the service instance\. Developers don't see the repository and can't change it\.
**Note**  
Developers creating services use the same process regardless of provisioning method, and the difference is abstracted from them\. However, with self\-managed provisioning developers might experience slower response, because they need to wait until someone \(which might not be themselves\) merges the PR in the infrastructure repository before provisioning can start\.
+ *Service with pipeline* – A service template for an environment with self\-managed provisioning may include a pipeline definition \(for example, an AWS CodePipeline pipeline\), written in Terraform HCL\. To enable AWS Proton to provision these pipelines, an administrator provides a linked pipeline repository to AWS Proton\. When provisioning a pipeline, the customer\-owned automation action in the repository should assume an IAM role with permissions to provision the pipeline, and an identity that reflects the destination AWS account\. The pipeline repository and role are separate from those used for each individual environment\. The linked repository is provided to AWS Proton separately, only once at the AWS account level, and it's used to provision and manage all pipelines\. The role should have permissions to create pipelines and other resources that your pipelines need\.

  The following procedures show how to provide the pipeline repository and role to AWS Proton\.

------
#### [ AWS Proton console ]

**To provide the pipeline role**

  1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), on the navigation pane, choose **Settings > Account settings**, and then choose **Configure**\.

  1. Use the **CI/CD pipeline repository** section to configure a new or existing repository link\.

------
#### [ AWS Proton API ]

**To provide the pipeline role**

  1. Use the [UpdateAccountSettings](https://docs.aws.amazon.com/proton/latest/APIReference/API_UpdateAccountSettings.html) API action\.

  1. Provide the provider, name, and branch of your pipeline repository in the `pipelineProvisioningRepository` parameter\.

------
#### [ AWS CLI ]

  **To provide the pipeline role**

  Run the following command:

  ```
  $ aws proton update-account-settings \
      --pipeline-provisioning-repository \
          "provider=GITHUB,name=my-pipeline-repo-name,branch=my-branch"
  ```

------
+ *Deletion of self\-managed provisioned resources* – Terraform modules may include configuration elements that are necessary for Terraform operation, in addition to resource definitions\. Therefore, AWS Proton can't delete all the Terraform files for an environment or service instance\. Instead, AWS Proton marks the files for deletion and updated a flag in the PR metadata\. Your automation can read that flag and use it to trigger a terraform destroy command\.