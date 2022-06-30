# AWS Proton environments<a name="ag-environments"></a>

For AWS Proton, an environment represents the set of shared resources and policies that AWS Proton [services](ag-services.md) are deployed into\. They can contain any resources that are expected to be shared across AWS Proton service instances\. These resources can include VPCs, clusters, and shared load balancers or API Gateways\. An AWS Proton environment must be created before a service can be deployed to it\.

This section describes how to manage environments using create, view, update, and delete operations\. For additional information, see the [https://docs.aws.amazon.com/proton/latest/APIReference/Welcome.html](https://docs.aws.amazon.com/proton/latest/APIReference/Welcome.html)\.

**Topics**
+ [IAM Roles](ag-environment-roles.md)
+ [Create an environment](ag-create-env.md)
+ [View environment data](ag-env-view.md)
+ [Update an environment](ag-env-update.md)
+ [Delete an environment](ag-env-delete.md)
+ [Environment account connections](ag-env-account-connections.md)