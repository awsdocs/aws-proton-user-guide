# What is AWS Proton?<a name="Welcome"></a>

**AWS Proton is:**
+ **Automated infrastructure as code provisioning and deployment of serverless and container\-based applications**

  The AWS Proton service is a two\-pronged automation framework\. As an administrator, you create *versioned service templates* that define standardized infrastructure and deployment tooling for serverless and container\-based applications\. As an application developer, you can select from the available *service templates* to automate your application or service deployments\.

  AWS Proton identifies all existing *service instances* that are using an outdated template version for you\. As an administrator, you can request AWS Proton to upgrade them with one click\.
+ **Standardized infrastructure**

  Platform teams can use AWS Proton and versioned infrastructure as code templates\. They can use these templates to define and manage standard application stacks that contain the architecture, infrastructure resources, and the CI/CD software deployment pipeline\.
+ **Deployments integrated with CI/CD**

  When developers use the AWS Proton self\-service interface to select a *service template*, they're selecting a standardized application stack definition for their code deployments\. AWS Proton automatically provisions the resources, configures the CI/CD pipeline, and deploys the code into the defined infrastructure\.

## AWS Proton for platform teams<a name="ag-admin"></a>

As an administrator, you or members of your platform team, create *environment templates* and *service templates* containing infrastructure as code\. The *environment template* defines shared infrastructure used by multiple applications or resources\. The *service template* defines the type of infrastructure that's needed to deploy and maintain a single application or microservice in an *environment*\. An AWS Proton *service* is an instantiation of a *service template*, which normally includes several *service instances* and a *pipeline*\. An AWS Proton *service instance* is an instantiation of a *service template* in a specific *environment*\. You or others in your team can specify which *environment templates* are compatible with a given *service template*\. For more information about *templates*, see [AWS Proton templates](ag-templates.md)\.

You can use the following infrastructure as code providers with AWS Proton:
+ [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
+ [Terraform](https://www.terraform.io/)

## AWS Proton for developers<a name="ag-dev"></a>

As an application developer, you select a standardized *service template* that AWS Proton uses to create a *service* that deploys and manages your application in a *service instance*\. An AWS Proton *service* is an instantiation of a *service template*, which normally includes several *service instances* and a *pipeline*\.

## AWS Proton workflow<a name="ag-workflow"></a>

The following diagram is a visualization of the main AWS Proton concepts discussed in the preceding paragraph\. It also offers a high\-level overview of what constitutes a simple AWS Proton workflow\.

![\[A diagram that describes the main AWS Proton concepts discussed in the preceding paragraph. It also offers a high-level overview of what constitutes a simple AWS Proton workflow divided into the following six steps.\]](http://docs.aws.amazon.com/proton/latest/userguide/images/service-diagram_1.png)

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-one.png)As an **Administrator**, you create and register an **Environment Template** with AWS Proton, which defines the shared resources\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-two.png)AWS Proton deploys one or more **Environments**, based on an **Environment Template**\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-three.png) As an **Administrator**, you create and register a **Service Template** with AWS Proton, which defines the related infrastructure, monitoring, and CI/CD resources as well as compatible **Environment Templates**\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-four.png) As a **Developer**, you select a registered **Service Template** and provide a link to your **Source code** repository\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-five.png) AWS Proton provisions the **Service** with a **CI/CD Pipeline** for your **Service instances**\.

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/proton/latest/userguide/images/label-six.png) AWS Proton provisions and manages the **Service** and the **Service Instances** that are running the **Source code** as was defined in the selected **Service Template**\. A **Service Instance** is an instantiation of the selected **Service Template** in an **Environment** for a single stage of a **Pipeline** \(for example Prod\)\.