--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# AWS Proton services<a name="ug-service"></a>

A Proton service is an instantiation of a service template, normally including several service instances and a pipeline\. A Proton service instance is an instantiation of a service template in a specific environment\. A service template is a complete definition of the infrastructure and pipeline for a Proton service\. Environments represent the set of shared resources and policies that Proton services are deployed into\. An environment applies to all stacks deployed for a service instance\. They can contain any resources that are expected to be shared across Proton service instances, such as VPCs, clusters, and shared load balancers or API Gateways\.

After your service instances are deployed, they may be updated by source code pushes that trigger the CI/CD pipeline or by the adoption of new versions of its service template\. Proton prompts you when new versions of its service template become available so you can update your services to a new version\. Proton re\-deploys the service and service instances when updates are invoked or triggered\.

**Topics**
+ [Create a service](ug-svc-create.md)
+ [View service data](ug-svc-view.md)
+ [Update a service](ug-svc-update.md)
+ [Delete](ug-svc-delete.md)
+ [View service instance data](ag-svc-instance-view.md)
+ [Update a service instance](ag-svc-instance-update.md)
+ [Update a service pipeline](ag-svc-pipeline-update.md)