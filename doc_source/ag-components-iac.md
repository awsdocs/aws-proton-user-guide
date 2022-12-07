# Component infrastructure as code files<a name="ag-components-iac"></a>

Component infrastructure as code \(IaC\) files are similar to those for other AWS Proton resources\. Learn here about some details that are specific to components\. For complete information about authoring IaC files for AWS Proton, see [Authoring templates and creating bundles for AWS Proton](ag-template-authoring.md)\.

## Using parameters with components<a name="ag-components-iac.namespace"></a>

The AWS Proton parameter namespace includes some parameters that a service IaC file can reference to get an associated component's name and outputs\. The namespace also includes parameters that a component IaC file can reference to get inputs, outputs, and resource values from the environment, service, and service instance that the component is associated with\.

A components doesn't have inputs of its own—it gets its inputs from the service instance it's attached to\. A component can also read environment outputs\.

For more information about using parameters in component and associated service IaC files, see [Component CloudFormation IaC file parameter details and examples](comp-parameters.md)\. For general information about AWS Proton parameters and a complete reference of the parameter namespace, see [AWS Proton parameters](parameters.md)\.

## Authoring robust IaC files<a name="ag-components-iac.robust"></a>

As an administrator, when you create a service template version, you can decide if you want to allow service instances created from the template version to have attached components\. See the [supportedComponentSources](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateServiceTemplateVersion.html#proton-CreateServiceTemplateVersion-request-supportedComponentSources) parameter of the [CreateServiceTemplateVersion](https://docs.aws.amazon.com/proton/latest/APIReference/API_CreateServiceTemplateVersion.html) API action in the *AWS Proton API Reference*\. However, for any future service instance, the person who creates the instance, decides whether or not to attach a component to it, and \(in the case of directly defined components\) authors the component IaC is typically a different person—a developer using your service template\. Therefore, you can't guarantee that a component would be attached to a service instance\. You also can't guarantee the existence of specific component output names or the validity and safety of the values of these outputs\.

AWS Proton and the Jinja syntax help you work around these issues and author robust service templates that render without failure in the following ways:
+ *AWS Proton parameter filters* – When you refer to component output properties, you can use *parameter filters*—modifiers that validate, filter, and format parameter values\. For more information and examples, see [Parameter filters for CloudFormation IaC files](parameter-filters.md)\.
+ *Single property default* – When you refer to a single resource or output property of a component, you can guarantee that rendering your service template won't fail by using the `default` filter, with or without a default value\. If the component, or a specific output parameter you're referring to, doesn't exist, the default value \(or an empty string, if you haven't specified a default value\) is rendered instead, and rendering succeeds\. For more information, see [Provide default values for nonexistent references](parameter-filters.md#parameter-filters.proton.default)\.

  Examples:
  + `{{ service_instance.components.default.name | default("") }}`
  + `{{ service_instance.components.default.outputs.my-output | default("17") }}`
**Note**  
Do not confuse the `.default` part of the namespace, which designates directly defined components, with the `default` filter, which provides a default value when referenced property doesn't exist\.
+ *Entire object reference* – When you refer to the entire component, or to the collection of a component's outputs, AWS Proton returns an empty object, `{}`, and therefore guarantees that rendering your service template won't fail\. You don't have to use any filter\. Be sure to make the reference in a context that can take an empty object, or use an `{{ if .. }}` condition to test for an empty object\.

  Examples:
  + `{{ service_instance.components.default }}`
  + `{{ service_instance.components.default.outputs }}`