# Component states<a name="ag-components-states"></a>

AWS Proton components can be in two fundamentally different states:
+ *Attached* – The component is attached to a service instance\. It defines infrastructure that supports the runtime functionality of the service instance\. The component extends the infrastructure defined in environment and service templates with developer\-defined infrastructure\.

  A typical component is in the attached state throughout most of the useful part of its lifecycle\.
+ *Detached* – The component is associated with an AWS Proton environment, and isn't attached to any service instance in the environment\.

  This is a transitional state for extending the lifetime of a component beyond a single service instance\.

The following table provides a top level comparison of the different component states\.


|  | Attached | Detached | 
| --- | --- | --- | 
|  **State's main purpose**  |  To extend the infrastructure of a service instance\.  |  To maintain the component's infrastructure between service instance attachments\.  | 
|  **Associated with**  |  A service instance and an environment  |  An environment  | 
|  **Key specific properties**  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/proton/latest/userguide/ag-components-states.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/proton/latest/userguide/ag-components-states.html)  | 
|  **Can be deleted**  |   ☓ No  |   ✓ Yes  | 
|  **Can be updated to another service instance**  |   ☓ No  |   ✓ Yes  | 
|  **Can read inputs**  |   ✓ Yes  |   ☓ No  | 

A component's main purpose is to be attached to a service instance and extend its infrastructure with additional resources\. An attached component can read inputs from the service instance according to the spec\. You can't directly delete the component or attach it to a different service instance\. You can't delete its service instance or the related service and environment, either\. To do any of these things, update the component to detach it from its service instance first\.

To maintain the component's infrastructure beyond the lifetime of a single service instance, you update the component and detach it from its service instance by removing the service and service instance names\. This detached state is a transitional state\. The component has no inputs\. Its infrastructure stays provisioned and you can update it\. You can delete resources that the component was associated with when it was attached \(service instance, service\)\. You can delete the component or update it to be attached to a service instance again\.