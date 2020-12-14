--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# What is AWS Proton<a name="Welcome"></a>

**AWS Proton is**
+ **Automated infrastructure provisioning and deployment of serverless and container\-based applications**

  The AWS Proton service is a two\-pronged automation framework\. Administrators create service templates to provide standardized infrastructure and deployment tooling for serverless and container\-based applications\. Then, you or other developers, in turn, can select from the available service templates to automate their application or service deployments\.
+ **Standardized infrastructure**

  Platform teams can use AWS Proton to define and manage standard application stacks that contain the architecture, infrastructure resources and the CI/CD software deployment pipeline\.
+ **Deployments integrated with CI/CD**

  When developers use the AWS Proton self\-service interface to select a service template, they're selecting a standardized application stack definition for their code deployments\. Proton automatically provisions the resources, configures the CI/CD pipeline and deploys the code into the defined infrastructure\.

   Proton identifies all existing service instances that are using an outdated template version for you\. As a developer, you can request Proton to update a service instance to the latest version of the service template\. 

## AWS Proton for developers<a name="ug-dev"></a>

As a developer, you select a standardized service template that Proton uses to build a service that deploys and manages your application as a Proton service\. A Proton service is an instantiation of a service template\.

The following diagram is a visualization of the main Proton concepts discussed in the preceding paragraph\. It also offers a high\-level overview of a simple Proton workflow\.

![\[A diagram that describes the main Proton concepts discussed in the preceding paragraph. It also offers a high-level overview of what constitutes a simple Proton workflow divided into the following six steps.\]](http://docs.aws.amazon.com/proton/latest/userguide/images/service-diagram_1.png)

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-one.png)As an **Administrator**, you create and register an **Environment Template** with Proton, which defines the shared resources\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-two.png)Proton deploys one or more **Environments**, based on an **Environment Template**\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-three.png)As an **Administrator**, you create and register a **Service Template** with Proton, which defines the related infrastructure, monitoring and CI/CD resources along with compatible **Environment Templates**\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-four.png)As a **Developer**, you select a registered **Service Template** and provide a link to your **Source code** repository\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-five.png) AWS Proton deploys the **Service** with a **CI/CD Pipeline** for your **Service instances**\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-six.png) AWS Proton deploys and manages the **Service** and the **Service Instances** that are running the **Source code** as was defined in the selected **Service Template**\. A **Service Instance** is an instantiation of the selected **Service Template** in an environment for a single stage of a **Pipeline** \(for example Prod\)\.