# Infrastructure security in AWS Proton<a name="infrastructure-security"></a>

As a managed service, AWS Proton is protected by the AWS global network security procedures that are described in the [Amazon Web Services: Overview of Security Processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

You use AWS published API calls to access AWS Proton through the network\. Clients must support Transport Layer Security \(TLS\) 1\.0 or later\. We recommend TLS 1\.2 or later\. Clients must also support cipher suites with perfect forward secrecy \(PFS\) such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Ephemeral Diffie\-Hellman \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\.

Additionally, requests must be signed by using an access key ID and a secret access key that is associated with an IAM principal\. Or you can use the [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) \(AWS STS\) to generate temporary security credentials to sign requests\.

To improve network isolation, you can use AWS PrivateLink as described in the following section\.

## AWS Proton and interface VPC endpoints \(AWS PrivateLink\)<a name="vpc-interface-endpoints"></a>

You can establish a private connection between your VPC and AWS Proton by creating an *interface VPC endpoint*\. Interface endpoints are powered by [AWS PrivateLink](http://aws.amazon.com/privatelink), a technology that enables you to privately access AWS Proton APIs without an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. Instances in your VPC don't need public IP addresses to communicate with AWS Proton APIs\. Traffic between your VPC and AWS Proton does not leave the Amazon network\. 

Each interface endpoint is represented by one or more [Elastic Network Interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) in your subnets\. 

For more information, see [Interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide*\. 

### Considerations for AWS Proton VPC endpoints<a name="vpc-endpoint-considerations"></a>

Before you set up an interface VPC endpoint for AWS Proton, ensure that you review [Interface endpoint properties and limitations](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-limitations) in the *Amazon VPC User Guide*\. 

AWS Proton supports making calls to all of its API actions from your VPC\.

VPC endpoint policies are supported for AWS Proton\. By default, full access to AWS Proton is allowed through the endpoint\. For more information, see [Controlling access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\.

### Creating an interface VPC endpoint for AWS Proton<a name="vpc-endpoint-create"></a>

You can create a VPC endpoint for the AWS Proton service using either the Amazon VPC console or the AWS Command Line Interface \(AWS CLI\)\. For more information, see [Creating an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) in the *Amazon VPC User Guide*\.

Create a VPC endpoint for AWS Proton using the following service name: 
+ com\.amazonaws\.*region*\.proton 

If you enable private DNS for the endpoint, you can make API requests to AWS Proton using its default DNS name for the Region, for example, `proton.region.amazonaws.com`\.

For more information, see [Accessing a service through an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#access-service-though-endpoint) in the *Amazon VPC User Guide*\.

### Creating a VPC endpoint policy for AWS Proton<a name="vpc-endpoint-policy"></a>

You can attach an endpoint policy to your VPC endpoint that controls access to AWS Proton\. The policy specifies the following information:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources on which actions can be performed\.

For more information, see [Controlling access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\. 

**Example: VPC endpoint policy for AWS Proton actions**  
The following is an example of an endpoint policy for AWS Proton\. When attached to an endpoint, this policy grants access to the listed AWS Proton actions for all principals on all resources\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Principal": "*",
      "Action": [
        "proton:ListServiceTemplates",
        "proton:ListServiceTemplateMajorVersions",
        "proton:ListServiceTemplateMinorVersions",
        "proton:ListServices",
        "proton:ListServiceInstances",
        "proton:ListEnvironments",
        "proton:GetServiceTemplate",
        "proton:GetServiceTemplateMajorVersion",
        "proton:GetServiceTemplateMinorVersion",
        "proton:GetService",
        "proton:GetServiceInstance",
        "proton:GetEnvironment",
        "proton:CreateService",
        "proton:UpdateService",
        "proton:UpdateServiceInstance",
        "proton:UpdateServicePipeline",
        "proton:DeleteService"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```