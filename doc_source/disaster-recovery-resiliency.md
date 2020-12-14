--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# Resilience in AWS Proton<a name="disaster-recovery-resiliency"></a>

The AWS global infrastructure is built around Regions and Availability Zones\. AWS Regions provide multiple, physically separated and isolated Availability Zones that are connected with low latency, high throughput, and highly redundant networking\. These Availability Zones offer you an effective way to design and operate applications and databases\. They are more highly available, fault tolerant, and scalable than traditional single data center infrastructures or multiple data center infrastructures\.

Each AWS Region has multiple Availability Zones\. You can deploy your applications across multiple Availability Zones in the same Region for fault tolerance and low latency\. Availability Zones are connected to each other with fast, private fiber\-optic networking, enabling you to easily architect applications that automatically fail over between Availability Zones without interruption\.

For more information about AWS Regions and Availability Zones, see [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)\.

In addition to the AWS global infrastructure, AWS Proton offers features to help support your data resiliency and backup needs\.

## AWS Proton backups<a name="backups"></a>

Proton maintains a backup of all customer data\. In the case of a total outage, this backup can be used to restore Proton and customer data from a previous valid state\.