# How AWS Proton provisions infrastructure<a name="ag-works-prov-methods"></a>

AWS Proton can provision infrastructure in two ways: *AWS\-managed provisioning* and *self\-managed provisioning*\. With AWS\-managed provisioning, AWS Proton calls the provisioning engine on your behalf\. With self\-managed provisioning, AWS Proton issues a pull request \(PR\) to a repository that you provide, where your own infrastructure deployment system runs the provisioning process\.

At this time, AWS\-managed provisioning supports AWS CloudFormation templates, and self\-managed provisioning supports Terraform templates\. For more information about the two Infrastructure as Code \(IaC\) template formats, see [AWS Proton infrastructure as code files](ag-infrastructure-tmp-files.md)\.

You choose the provisioning method at the environment level\. When you create or update an environment, you choose if it uses AWS\-managed or self\-managed provisioning\. You then provide the required parameters — for example, an AWS Identity and Access Management \(IAM\) role or the infrastructure repository\. All service instances deployed to the environment use the same provisioning method as the environment itself\. You can't change the provisioning method at the instance level\.

Developers who use AWS Proton to provision a service have the same experience in both cases\. Developers don't need to be aware of the provisioning method and don't need to change anything in the service provisioning process\. The environment that they choose to deploy their service to determines the provisioning behavior\.

## How AWS\-managed provisioning works<a name="ag-works-prov-methods-direct"></a>

When an environment is configured to use AWS\-managed provisioning, infrastructure is provisioned as follows:

1. An AWS Proton customer \(an administrator or a developer\) creates the AWS Proton resource \(an environment or a service\)\. The customer selects a template for the resource and provides the required parameters\. For an environment, the customer also provides a role\. For more information, see the following section, [Considerations for AWS\-managed provisioning](#ag-works-prov-methods-direct-consider)\.

1. AWS Proton renders a complete AWS CloudFormation template for provisioning the resource\.

1. AWS Proton calls AWS CloudFormation to start provisioning using the rendered template\.

1. AWS Proton continuously monitors the AWS CloudFormation deployment\.

1. When provisioning completes, AWS Proton reports back errors in case of failure, and captures provisioning outputs, like Amazon VPC ID, in case of success\.

The following diagram shows that AWS Proton takes care of most of these steps directly\.

![\[Diagram illustrating AWS-managed provisioning in AWS Proton\]](http://docs.aws.amazon.com/proton/latest/userguide/images/provisioning-direct.png)

### Considerations for AWS\-managed provisioning<a name="ag-works-prov-methods-direct-consider"></a>
+ *Infrastructure provisioning role* – When an administrator configures an environment for AWS\-managed provisioning, they need to provide an IAM role \(either directly or as part of an AWS Proton environment account connection\)\. AWS Proton uses this role to provision the infrastructure of the environment and all the service instances that are deployed to it\. The role should have permissions to create all the resources that your environment and service templates include\.
+ *Service provisioning* – When a developer chooses an environment with AWS\-managed provisioning for their service instance, AWS Proton uses the role provided to that environment to provision infrastructure for the service instance\. Developers don't see this role and can't change it\.
+ *Service with pipeline* – A service template for an environment with AWS\-managed provisioning may include a pipeline definition \(for example, an AWS CodePipeline pipeline\), written in the AWS CloudFormation YAML schema\. AWS Proton also creates the pipeline by calling AWS CloudFormation\. The role that AWS Proton uses to create a pipeline is separate from the role for each individual environment\. This role is provided to Proton AWS Proton separately, only once at the AWS account level, and it's used to provision and manage all pipelines\. This role should have permissions to create pipelines and other resources that your pipelines need\.

  The following procedures show how to provide the pipeline role to AWS Proton\.

------
#### [ AWS Proton console ]

**To provide the pipeline role**

  1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), on the navigation pane, choose **Settings > Account settings**, and then choose **Configure**\.

  1. Use the **CI/CD pipeline roles** section to configure a new or existing pipeline service role\.

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
+ *Service provisioning* – When a developer chooses an environment with self\-managed provisioning for their service instance, AWS Proton submits a PR to the associated repository to provision infrastructure for the service instance\. Developers don't see the repository and can't change it\.
**Note**  
Developers creating services in an environment use the same process no matter which provisioning method the environment uses, and the difference is abstracted from them\. However, with self\-managed provisioning developers might experience slower response, because they need to wait until someone \(which might not be themselves\) merges the PR in the infrastructure repository before provisioning can start\.
+ *Service with pipeline* – A service template for an environment with self\-managed provisioning may include a pipeline definition \(for example, an AWS CodePipeline pipeline\), written in Terraform HCL\. To enable AWS Proton to provision these pipelines, an administrator provides a linked pipeline repository to AWS Proton\. When provisioning a pipeline, the customer\-owned automation action in the repository should assume an IAM role with permissions to provision the pipeline, and an identity that reflects the destination AWS account\. The pipeline repository and role are separate from those used for each individual environment\. The linked repository is provided to Proton AWS Proton separately, only once at the AWS account level, and it's used to provision and manage all pipelines\. The role should have permissions to create pipelines and other resources that your pipelines need\.

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