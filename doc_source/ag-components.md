# AWS Proton components<a name="ag-components"></a>

Components are a type of AWS Proton resource\. They add flexibility to service templates\. Components provide platform teams with a mechanism to extend core infrastructure patterns, and define safeguards that empower developers to manage aspects of their application infrastructure\.

In AWS Proton administrators define standard infrastructure that is used across development teams and applications\. However, development teams might need to include additional resources for their specific use cases, like Amazon Simple Queue Service \(Amazon SQS\) queues or Amazon DynamoDB tables\. These application\-specific resources might change frequently, particularly during early application development\. Maintaining these frequent changes in administrator authored templates might be hard to manage and scale—administrators would need to maintain many more templates without real administrator added value\. The alternative—letting application developers author templates for their applications—isn't ideal either, because it takes away administrators' ability to standardize the main architecture components, like AWS Fargate tasks\. This is where components come in\.

With a component, a developer can add supplemental resources to their application, above and beyond what administrators defined in environment and service templates\. The developer then attaches the component to a service instance\. AWS Proton provisions infrastructure resources defined by the component just like it provisions resources for environments and service instances\.

A component can read service instance inputs and provide outputs to the service instance, for a fully integrated experience\. For example, if the component adds an Amazon Simple Storage Service \(Amazon S3\) bucket for use by a service instance, the component template can take the environment and service instance names into account for naming the bucket\. When AWS Proton renders the service template to provision a service instance, the service instance can refer to the bucket and use it\.

The components that AWS Proton currently supports are *directly defined components*\. You pass the Infrastructure as Code \(IaC\) file that defines the component's infrastructure directly to the AWS Proton API or console\. This is different than an environment or service, where you define IaC in a template bundle and register the bundle as a template resource, then use a template resource to create the environment or service\.

**Note**  
Directly defined components allow developers to define extra infrastructure and provision it\. AWS Proton provisions all directly defined components running in the same environment using the same AWS Identity and Access Management \(IAM\) role\.

An administrator can control what developers can do with components in two ways:
+ *Supported component sources* – An administrator can allow the attachment of components to service instances based on a property of AWS Proton service template versions\. By default, developers can't attach components to service instances\.

  For more information about this property, see the [supportedComponentSources](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateServiceTemplateVersion.html#proton-CreateServiceTemplateVersion-request-supportedComponentSources) parameter of the [CreateServiceTemplateVersion](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateServiceTemplateVersion.html) API action in the *AWS Proton API Reference*\.
**Note**  
When you use template sync, AWS Proton creates service template versions implicitly when you commit changes to a service template bundle in a repository\. In this case, instead of specifying supported component sources during service template version creation, you specify this property in a file associated with each service template major version\. For more information, see [Syncing service templates](create-template-sync.md#create-template-sync-service-templates)\.
+ *Component roles* – An administrator can assign a component role to an environment\. AWS Proton assumes this role when it provisions infrastructure defined by directly defined component in the environment\. Therefore, the component role scopes down the infrastructure that developers can add using directly defined components in the environment\. In the absence of the component role, developers can't create directly defined components in the environment\.

  For more information about assigning a component role, see the [componentRoleArn](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateEnvironment.html#proton-CreateEnvironment-request-componentRoleArn) parameter of the [CreateEnvironment](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateEnvironment.html) API action in the *AWS Proton API Reference*\.
**Note**  
Component roles are used only in [AWS\-managed provisioning](ag-works-prov-methods.md#ag-works-prov-methods-direct) environments\. They aren't used in [Self\-managed provisioning](ag-works-prov-methods.md#ag-works-prov-methods-self) environments\.

**Topics**
+ [How do components compare to other AWS Proton resources?](#ag-components.compare)
+ [Components in the AWS Proton console](#ag-components.console)
+ [Components in the AWS Proton API and AWS CLI](#ag-components.api)
+ [Component frequently asked questions](#ag-components.faq)
+ [Component states](ag-components-states.md)
+ [Component infrastructure as code files](ag-components-iac.md)
+ [Component AWS CloudFormation example](ag-components-example-cfn.md)

## How do components compare to other AWS Proton resources?<a name="ag-components.compare"></a>

In many ways, components are similar to other AWS Proton resources\. Their infrastructure is defined in an [IaC template file](ag-components-iac.md), authored in either AWS CloudFormation YAML or Terraform HCL format\. AWS Proton can provision component infrastructure using either [AWS\-managed provisioning](ag-works-prov-methods.md#ag-works-prov-methods-direct) or [self\-managed provisioning](ag-works-prov-methods.md#ag-works-prov-methods-self)\.

Components are, however, different from other AWS Proton resources in a few ways:
+ *Detached state* – Components are designed to be attached to service instances and to extend their infrastructure, but can also be in a *detached* state, in which they aren't attached to any service instance\. For more information about component states, see [Component states](ag-components-states.md)\.
+ *No schema* – Components don't have an associated schema like [template bundles](ag-template-authoring.md#ag-template-bundles) have\. Component inputs are defined by a service\. A component can consume inputs when it is attached to a service instance\.
+ *No customer\-managed components* – AWS Proton always provisions component infrastructure for you\. There isn't a *bring your own resources* version of components\. For more information about customer\-managed environments, see [Create an environment](ag-create-env.md)\.
+ *No template resource* – Directly defined components don't have an associated template resource similar to environment and service templates\. You provide an IaC template file directly to the component\. Similarly, you directly provide a manifest that defines the template language and rendering engine for provisioning the component's infrastructure\. You author the template file and the manifest in a way similar to authoring a [template bundle](ag-template-authoring.md#ag-template-bundles)\. However, with directly defined components, there's no requirement to store IaC files as bundles in particular locations, and you don't create a template resource in AWS Proton out of IaC files\.

## Components in the AWS Proton console<a name="ag-components.console"></a>

Use the AWS Proton console to create, update, view, and use AWS Proton components\.

The following console pages are related to components\. We include direct links to top level console pages\.
+ [Components](https://console.aws.amazon.com/proton/#/components) – View the list of components in your AWS account\. You can create new components, and update or delete existing components\. Choose a component name on the list to view its details page\.

  Similar lists exist also on the **Environment details** and **Service instance details** pages\. These lists show only the components associated with the resource that is being viewed\. When you create a component from one of these lists, AWS Proton pre\-selects the associated environment on the **Create component** page\.
+ **Component details** – To view the component details page, choose a component name on the [Components](https://console.aws.amazon.com/proton/#/components) list\.

  On the details page, view the component details and status, and update or delete the component\. View and manage lists of outputs \(for example, provisioned resource ARNs\), provisioned AWS CloudFormation stacks, and assigned tags\.
+ [Create component](https://console.aws.amazon.com/proton/#/components/create) – Create a component\. Enter the component name and description, choose the associated resources, specify the component source IaC file, and assign tags\.
+ **Update component** – To update a component, select the component on the [Components](https://console.aws.amazon.com/proton/#/components) list, and then, on the **Actions** menu, choose **Update component**\. Alternatively, on the **Component details** pages, choose **Update**\.

  You can update most of the component's details\. You can't update the component name\. And you can choose whether or not to redeploy the component after a successful update\.
+ **Configure environment** – When you create or update an environment, you can specify a **Component role**\. This role controls the ability to run directly defined components in the environment and provides permissions for provisioning them\.
+ **Create new service template version** – When you create a service template version, you can specify **Supported component sources** for the template version\. This controls the ability to attach components to service instances of services based on this template version\.

## Components in the AWS Proton API and AWS CLI<a name="ag-components.api"></a>

Use the AWS Proton API or the AWS CLI to create, update, view, and use AWS Proton components\.

The following API actions directly manage AWS Proton component resources\.
+ [CreateComponent](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateComponent.html) – Create an AWS Proton component\.
+ [DeleteComponent](https://docs.aws.amazon.com/proton/latest/APIReference/API_DeleteComponent.html) – Delete an AWS Proton component\.
+ [GetComponent](https://docs.aws.amazon.com/proton/latest/APIReference/API_GetComponent.html) – Get detailed data for a component\.
+ [ListComponentOutputs](https://docs.aws.amazon.com/proton/latest/APIReference/API_ListComponentOutputs.html) – Get a list of component Infrastructure as Code \(IaC\) outputs\.
+ [ListComponentProvisionedResources](https://docs.aws.amazon.com/proton/latest/APIReference/API_ListComponentProvisionedResources.html) – List provisioned resources for a component with details\.
+ [ListComponents](https://docs.aws.amazon.com/proton/latest/APIReference/API_ListComponents.html) – List components with summary data\. You can filter the result list by environment, service, or a single service instance\.

The following API actions of other AWS Proton resources have some functionality related to components\.
+ [CreateEnvironment](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateEnvironment.html), [UpdateEnvironment](https://docs.aws.amazon.com/proton/latest/APIReference/API_UpdateEnvironment.html) – Use `componentRoleArn` to specify the Amazon Resource Name \(ARN\) of the IAM service role that AWS Proton uses when provisioning directly defined components in this environment\. It determines the scope of infrastructure that a directly defined component can provision\.
+ [CreateServiceTemplateVersion](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateServiceTemplateVersion.html) – Use `supportedComponentSources` to specify supported component sources\. Components with supported sources can be attached to service instances based on this service template version\.

## Component frequently asked questions<a name="ag-components.faq"></a>

**What is the lifecycle of a component?**

Components can be in an *attached* or *detached* state\. They are designed to be attached to a service instance and enhance its infrastructure most of the time\. Detached components are in a transitional state that enables you to delete a component or attach it to another service instance in a controlled and safe way\. For more information, see [Component states](ag-components-states.md)\.

**Why can't I delete my attached components?**

*Solution:* To delete an attached component, update the component to detach it from the service instance, validate service instance stability, and then delete the component\.

*Why is this required?* Attached components provide extra infrastructure that your application needs to perform its runtime functions\. The service instance might be using component outputs to detect and use resources of this infrastructure\. Deleting the component, thereby removing its infrastructure resources, could be disruptive to the attached service instance\.

As an added safety measure, AWS Proton requires that you update the component and detach it from its service instance before you can delete it\. You can then validate your service instance to ensure that it continues to deploy and function properly\. If you detect an issue, you can quickly reattach the component to the service instance, then work to fix the issue\. When you're confident that your service instance is clear of any dependency on the component, you can safely delete the component\.

**Why can't I change a component's attached service instance directly?**

*Solution:* To change attachment, update the component to detach it from the service instance, validate component and service instance stability, then attach the component to the new service instance\.

*Why is this required?* A component is designed to be attached to a service instance\. Your component might use service instance inputs for infrastructure resource naming and configuration\. Changing the attached service instance could be disruptive to the component \(in addition to possible disruption to the service instance, as described in the previous FAQ, [Why can't I delete my attached components?](#ag-components.faq.delete)\)\. For example, it might cause renaming, and possibly even replacement, of resources defined in the component's IaC template\.

As an added safety measure, AWS Proton requires that you update the component and detach it from its service instance before you can attach it to another service instance\. You can then validate the stability of both the component and the service instance before attaching the component to the new service instance\.