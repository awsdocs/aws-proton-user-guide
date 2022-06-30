# Automate AWS Proton with EventBridge<a name="event-bridge"></a>

You can monitor AWS Proton events in Amazon EventBridge\. EventBridge delivers a stream of real\-time data from your own applications, software\-as\-a\-service \(SaaS\) applications, and AWS services\. You can configure events to respond to AWS resource state changes\. EventBridge routes this data then to *target* services such as AWS Lambda and Amazon Simple Notification Service\. These events are the same as those that appear in Amazon CloudWatch Events\. CloudWatch Events delivers a near real\-time stream of system events that describe changes in AWS resources\. For more information, see [What Is Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/) in the *Amazon EventBridge User Guide*\.

Use EventBridge to be notified of state changes in the AWS Proton provisioning workflows\.

## Event types<a name="event-types"></a>

Events are composed of rules that include an event pattern and targets\. You configure a rule by choosing event pattern and target objects:

Event pattern  
Each rule is expressed as an event pattern with the source and type of events to monitor and the event targets\. To monitor events, you create a rule with the service that you're monitoring as the event source\. For example, you can create a rule with an event pattern that uses AWS Proton as an event source to trigger a rule when there are changes in a deployment state\.

Targets  
The rule receives a selected service as the event target\. You can set up a target service to send notifications, capture state information, take corrective action, initiate events, or take other actions\.

Event objects contain standard fields of ID, account, AWS Region, detail\-type, source, version, resource, time \(optional\)\. The detail field is a nested object containing custom fields for the event\.

AWS Proton events are emitted on a best effort basis\. Best effort delivery means that the service attempts to send all events to EventBridge, but in some rare cases an event might not be delivered\.

For each AWS Proton resource that can emit events, the following table lists the detail\-type value, detail fields, and \(where available\) a reference to a list of values for the `status` and `previousStatus` detail fields\. When a resource is deleted, the `status` detail field value is `DELETED`\.


| Resource | Detail\-type value | Detail fields | 
| --- | --- | --- | 
|  `EnvironmentTemplate`  |  AWS Proton Environment Template Status Change  |  `name` `status` `previousStatus`  | 
|  `EnvironmentTemplateVersion`  |  AWS Proton Environment Template Version Status Change  |  `name` `majorVersion` `minorVersion` `status` `previousStatus` [status values](https://docs.aws.amazon.com/proton/latest/APIReference/API_EnvironmentTemplateVersion.html#proton-Type-EnvironmentTemplateVersion-status)  | 
|  `ServiceTemplate`  |  AWS Proton Service Template Status Change  |  `name` `status` `previousStatus`  | 
|  `ServiceTemplateVersion`  |  AWS Proton Service Template Version Status Change  |  `name` `majorVersion` `minorVersion` `status` `previousStatus` [status values](https://docs.aws.amazon.com/proton/latest/APIReference/API_ServiceTemplateVersion.html#proton-Type-ServiceTemplateVersion-status)  | 
|  `Environment`  |  AWS Proton Environment Status Change  |  `name` `status` `previousStatus`  | 
|  `Service`  |  AWS Proton Service Status Change  |  `name` `status` `previousStatus` [status values](https://docs.aws.amazon.com/proton/latest/APIReference/API_Service.html#proton-Type-Service-status)  | 
|  `ServiceInstance`  |  AWS Proton Service Instance Status Change  |  `name` `serviceName` `status` `previousStatus`  | 
|  `ServicePipeline`  |  AWS Proton Service Pipeline Status Change  |  `serviceName` `status` `previousStatus`  | 
|  `EnvironmentAccountConnection`  |  AWS Proton Environment Account Connection Status Change  |  `id` `status` `previousStatus` [status values](https://docs.aws.amazon.com/proton/latest/APIReference/API_EnvironmentAccountConnection.html#proton-Type-EnvironmentAccountConnection-status)  | 
|  `Component`  |  AWS Proton Component Status Change  |  `name` `status` `previousStatus`  | 

## AWS Proton event examples<a name="event-examples"></a>

The following examples show the ways that AWS Proton can send events to EventBridge\.

**Service template**

```
{
    "source": "aws.proton",
    "detail-type": ["AWS Proton Service Template Status Change"],
    "time": "2021-03-22T23:21:40.734Z",
    "resources": ["arn:aws:proton:region_id:123456789012:service-template/sample-service-template-name"],
    "detail": {
        "name": "sample-service-template-name",
        "status": "PUBLISHED",
        "previousStatus": "DRAFT"
    }
}
```

**Service template version**

```
{
    "source": "aws.proton",
    "detail-type": ["AWS Proton Service Template Version Status Change"],
    "time": "2021-03-22T23:21:40.734Z",
    "resources": ["arn:aws:proton:region_id:123456789012:service-template/sample-service-template-name:1.0"],
    "detail": {
        "name": "sample-service-template-name",
        "majorVersion": "1",
        "minorVersion": "0",
        "status": "REGISTRATION_FAILED",
        "previousStatus": "REGISTRATION_IN_PROGRESS"
    }
}
```

**Environment**

```
{
    "source": "aws.proton",
    "detail-type": ["AWS Proton Environment Status Change"],
    "time": "2021-03-22T23:21:40.734Z",
    "resources": ["arn:aws:proton:region_id:123456789012:environment/sample-environment"],
    "detail": {
        "name": "sample-environment",
        "status": "DELETE_FAILED",
        "previousStatus": "DELETE_IN_PROGRESS"
    }
}
```