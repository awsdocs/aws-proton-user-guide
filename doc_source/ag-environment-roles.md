# IAM Roles<a name="ag-environment-roles"></a>

With AWS Proton, you supply the IAM roles and AWS KMS keys for the AWS resources that you own and manage\. These are later applied to and used by resources owned and managed by developers\. You create an IAM role to control your developer team's access to the AWS Proton API\.

## AWS Proton service role<a name="ag-environment-roles.service-role"></a>

When you create a new environment, you provide a related IAM service role\. The role contains all permissions that are necessary to update all provisioned infrastructure defined in both the environment templates and the service templates\. For role examples, see [AWS Proton service role for provisioning using AWS CloudFormation](security_iam_service-role-policy-examples.md#proton-svc-role)\. If you use environment account connections and environment accounts, you create the role in a selected environment account\. For more information, see [Create an environment in one account and provision in another account](ag-create-env.md#ag-create-env-deploy-other) and [Environment account connections](ag-env-account-connections.md)\.

How you provide this service role, and who assumes the role, depends on your environment's provisioning method\.
+ *AWS\-managed provisioning* – You provide the role to AWS Proton, either directly while creating an environment, or indirectly through account connections\. AWS Proton assumes the role in the relevant account to provision environment and service infrastructure\.
+ *Self\-managed provisioning* – It's your responsibility to configure your provisioning automation to assume an appropriate role using appropriate credentials when a pull request \(PR\) triggers a provisioning action\. For an example GitHub Action that assumes a role, see [Assuming a Role](https://github.com/aws-actions/configure-aws-credentials#assuming-a-role) in the *"Configure AWS Credentials" Action For GitHub Actions* documentation\.

For more information about provisioning methods, see [How AWS Proton provisions infrastructure](ag-works-prov-methods.md)\.