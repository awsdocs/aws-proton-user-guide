# AWS Proton services<a name="ug-service"></a>

An AWS Proton service is an instantiation of a service template, normally including several service instances and a pipeline\. An AWS Proton service instance is an instantiation of a service template in a specific environment\. A service template is a complete definition of the infrastructure and optional service pipeline for an AWS Proton service\.

Environments represent the set of shared resources and policies that AWS Proton services are deployed into\. They can contain any resources that are expected to be shared across AWS Proton service instances, such as VPCs, clusters, and shared load balancers or API Gateways\.

After your service instances are deployed, they can be updated by source code pushes that prompt the CI/CD pipeline or by the adoption of new versions of its service template\. AWS Proton prompts you when new versions of its service template become available so you can update your services to a new version\. When your service is updated, AWS Proton re\-deploys the service and service instances\.

The following sections cover the use of AWS Proton service create, view, edit and delete operations\. For additional information, see the [https://docs.aws.amazon.com/proton/latest/APIReference/Welcome.html](https://docs.aws.amazon.com/proton/latest/APIReference/Welcome.html)\.

**Topics**
+ [Create a service](ug-svc-create.md)
+ [View service data](ug-svc-view.md)
+ [Edit a service](ug-svc-update.md)
+ [Delete](ug-svc-delete.md)
+ [View service instance data](ag-svc-instance-view.md)
+ [Update a service instance](ag-svc-instance-update.md)
+ [Update a service pipeline](ag-svc-pipeline-update.md)