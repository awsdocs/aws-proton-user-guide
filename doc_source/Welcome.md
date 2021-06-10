# What is AWS Proton<a name="Welcome"></a>

**AWS Proton is**
+ **Automated infrastructure provisioning and deployment of serverless and container\-based applications**

  The AWS Proton service is a two\-pronged automation framework\. Administrators create versioned service templates that define standardized infrastructure and deployment tooling for serverless and container\-based applications\. Then, you or other developers, in turn, can select from the available service templates to automate application or service deployments\.

  AWS Proton identifies identifies all existing service instances that are using an outdated template version for you\. As a developer, you can request AWS Proton to update a service instance to the latest version of the service template\.
+ **Standardized infrastructure**

  Platform teams can use AWS Proton and versioned infrastructure\-as\-code templates to define and manage standard application stacks that contain the architecture, infrastructure resources and the CI/CD software deployment pipeline\.
+ **Deployments integrated with CI/CD**

  When developers use the AWS Proton self\-service interface to select a service template, they're selecting a standardized application stack definition for their code deployments\. AWS Proton automatically provisions the resources, configures the CI/CD pipeline and deploys the code into the defined infrastructure\.

## AWS Proton for developers<a name="ug-dev"></a>

As a developer, you select a standardized service template that AWS Proton uses to build a service that deploys and manages your application as an AWS Proton service\. An AWS Proton service is an instantiation of a service template\.

The following diagram is a visualization of the main AWS Proton concepts discussed in the preceding paragraph\. It also offers a high\-level overview of a simple AWS Proton workflow\.

![\[A diagram that describes the main AWS Proton concepts discussed in the preceding paragraph. It also offers a high-level overview of what constitutes a simple AWS Proton workflow divided into the following six steps.\]](http://docs.aws.amazon.com/proton/latest/userguide/images/service-diagram_1.png)

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-one.png)As an **Administrator**, you create and register an **Environment Template** with AWS Proton, which defines the shared resources\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-two.png)AWS Proton deploys one or more **Environments**, based on an **Environment Template**\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-three.png)As an **Administrator**, you create and register a **Service Template** with AWS Proton, which defines the related infrastructure, monitoring and CI/CD resources along with compatible **Environment Templates**\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-four.png)As a **Developer**, you select a registered **Service Template** and provide a link to your **Source code** repository\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-five.png) AWS Proton deploys the **Service** with a **CI/CD Pipeline** for your **Service instances**\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-six.png) AWS Proton deploys and manages the **Service** and the **Service Instances** that are running the **Source code** as was defined in the selected **Service Template**\. A **Service Instance** is an instantiation of the selected **Service Template** in an **Environment** for a single stage of a **Pipeline** \(for example Prod\)\.