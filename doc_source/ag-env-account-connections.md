# Environment account connections<a name="ag-env-account-connections"></a>

**Overview**

Learn how to create and manage an AWS Proton environment in one account and provision its infrastructure resources in another account\. This can help improve visibility and efficiency at scale\. Environment account connections only support standard provisioning with AWS CloudFormation infrastructure as code\.

**Note**  
The information provided in this topic is relevant to environments configured with *AWS\-managed provisioning*\. With environments configured with *self\-managed provisioning*, AWS Proton doesn't directly provision your infrastructure\. Instead, it sends pull requests \(PRs\) to your repository for provisioning\. It's your responsibility to ensure that your automation code assumes the right identity and role\.  
For more information about provisioning methods, see [How AWS Proton provisions infrastructure](ag-works-prov-methods.md)\.

**Terminology**

![\[A diagram that describes AWS Proton resources within a single account (management account) that's in a single AWS Region. It also shows how AWS Proton environments in that account can use environment account connections to deploy to other accounts (environment accounts) in the same Region.\]](http://docs.aws.amazon.com/proton/latest/userguide/images/xaccount-diagram.png)

With AWS Proton *environment account connections*, you can create an AWS Proton environment from one account and provision it's infrastructure in another account\.

Management account  
The single account where you, as an administrator, create an AWS Proton environment that provisions infrastructure resources in another *environment account*\.

Environment account  
An account that environment infrastructure is provisioned in, when you create an AWS Proton environment in another account\.

Environment account connection  
A secure bi\-directional connection between a *management account* and an *environment account*\. It maintains authorization and permissions as described further in the following sections\.

When you create an environment account connection in an environment account, in a specific Region, only the management accounts in the same Region can see and use the environment account connection\. This means that the AWS Proton environment created in the management account and the environment infrastructure provisioned in the environment account must be in the same Region\.

**Environment account connection considerations**
+ You need an environment account connection for each environment that you want to provision in an environment account\.
+ For information about environment account connection quotas, see [AWS Proton quotas](ag-limits.md)\.

**Tagging**

In the environment account, use the console or the AWS CLI to view and manage environment account connection customer managed tags\. AWS managed tags *aren't* generated for environment account connections\. For more information, see [AWS Proton resources and tagging](resources.md)\.

## Create an environment in one account and provision its infrastructure in another account<a name="ag-env-account-connections-create-env"></a>

To create and provision an environment from a single management account, set up an environment account for an environment that you plan to create\. 

**Start in the environment account and create connection\.**

In the environment account, create an AWS Proton service role that's scoped down to only the permissions that are needed for provisioning your environment infrastructure resources\. For more information, see [AWS Proton service role](security_iam_service-role-policy-examples.md#proton-svc-role)\.

Then create and send an environment account connection request to your management account\. When the request is accepted, AWS Proton can use the associated IAM role that permits environment resource provisioning in the associated environment account\.

**In the management account, accept or reject the environment account connection\.**

In the management account, accept or reject the environment account connection request\. You *can’t* delete an environment account connection from your management account\.

If you accept the request, the AWS Proton can use the associated IAM role that permits resource provisioning in the associated environment account\.

The environment infrastructure resources are provisioned in the associated environment account\. You can only use AWS Proton APIs to access and manage your environment and its infrastructure resources, from your management account\. For more information, see [Create an environment in one account and provision in another account](ag-create-env.md#ag-create-env-deploy-other) and [Update an environment](ag-env-update.md)\.

After you reject a request, you *can’t* accept or use the rejected environment account connection\.

**Note**  
You *can’t* reject an environment account connection that's connected to an environment\. To reject the environment account connection, you must first delete the associated environment\.

**In the environment account, access the provisioned infrastructure resources\.**

In the environment account, you can view and access the provisioned infrastructure resources\. For example, you can use CloudFormation API actions to monitor and clean up stacks if needed\. You can’t use the AWS Proton API actions to access or manage the AWS Proton environment that was used to provision the infrastructure resources\.

In the environment account, you can delete environment account connections that you have created in the environment account\. You *can’t* accept or reject them\. If you delete an environment account connection that’s in use by an AWS Proton environment, AWS Proton *won’t* be able to manage the environment infrastructure resources until a new environment connection is accepted for the environment account and named environment\. You're responsible for cleaning up provisioned resources that remain without an environment connection\.

## Use the console or CLI to manage environment account connections<a name="ag-env-account-connections-create-env-console"></a>

You can use the console or CLI to create and manage environment account connections\.

------
#### [ AWS Management Console ]

**Use the console to create an environment account connection and send a request to the management account as shown in the next steps\.**

1. Decide on a name for the environment that you plan to create in your management account or choose the name of an existing environment that requires an environment account connection\.

1. In an environment account, in the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environment account connections** in the navigation pane\.

1. In the **Environment account connections** page, choose **Request to connect**\.
**Note**  
Verify the account ID that's listed in the **Environment account connection** page heading\. Make sure that it matches the account ID of the environment account that you want your named environment to provision in\.

1. In the **Request to connect** page:

   1. In the **Connect to management account** section, enter the **Management account ID** and the **Environment name** that you entered in step 1\.

   1. In the **Environment role** section, choose **New service role** and AWS Proton automatically creates a new role for you\. Or, select **Existing service role** and the name of the service role that you created previously\.
**Note**  
The role that AWS Proton automatically creates for you has broad permissions\. We recommend that you scope down the role to the permissions required to provision your environment infrastructure resources\. For more information, see [AWS Proton service role](security_iam_service-role-policy-examples.md#proton-svc-role)\.

   1. \(Optional\) In the **Tags** section, choose **Add new tag** to create a customer managed tag for your environment account connection\.

   1. Choose **Request to connect**\.

1. Your request shows as pending in the **Environment connections sent to a management account** table and a modal lets you know how to accept the request from the management account\.

**Accept or reject an environment account connection request\.**

1. In a management account, in the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environment account connections** in the navigation pane\.

1. In the **Environment account connections** page, in the **Environment account connection requests** table, choose the environment connection request to accept or reject\.
**Note**  
Verify the account ID that's listed in the **Environment account connection** page heading\. Make sure that it matches the account ID of the management account that's associated with the environment account connection to reject\. After you reject this environment account connection, you *can’t* accept or use the rejected environment account connection\.

1. Choose **Reject** or **Accept**\.
   + If you selected **Reject**, the status changes from *pending* to *rejected*\.
   + If you selected **Accept**, the status changes from *pending* to *connected*\.

**Delete an environment account connection\.**

1. In an environment account, in the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environment account connections** in the navigation pane\.
**Note**  
Verify the account ID that's listed in the **Environment account connection** page heading\. Make sure that it matches the account ID of the management account that's associated with the environment account connection to reject\. After you delete this environment account connection, AWS Proton *can’t* manage the environment infrastructure resources in the environment account\. It can only manage it after a new environment account connection for the environment account and named environment is accepted by the management account\.

1. In the **Environment account connections** page, in the **Sent requests to connect to management account** section, choose **Delete**\.

1. A modal prompts you to confirm the deletion\. Choose **Delete**\.

------
#### [ AWS CLI ]

Decide on a name for the environment that you plan to create in your management account or choose the name of an existing environment that requires an environment account connection\.

**Create an environment account connection in an environment account\.**

Run the following command:

```
$ aws proton create-environment-account-connection \
    --environment-name "simple-env-connected" \
    --role-arn "arn:aws:iam::123456789222:role/service-role/env-account-proton-service-role" \
    --management-account-id "123456789111"
```

Response:

```
{
    "environmentAccountConnection": {
        "arn": "arn:aws:proton:region-id:123456789222:environment-account-connection/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "environmentAccountId": "123456789222",
        "environmentName": "simple-env-connected",
        "id": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "lastModifiedAt": "2021-04-28T23:13:50.847000+00:00",
        "managementAccountId": "123456789111",
        "requestedAt": "2021-04-28T23:13:50.847000+00:00",
        "roleArn": "arn:aws:iam::123456789222:role/service-role/env-account-proton-service-role",
        "status": "PENDING"
    }
}
```

**Accept or reject an environment account connection in a management account as shown in the following command and response\.**

**Note**  
If you reject this environment account connection, you *won’t* be able to accept or use the rejected environment account connection\.

If you specify **Reject**, the status changes from *pending* to *rejected*\.

If you specify **Accept**, the status changes from *pending* to *connected*\.

Run the following command to accept the environment account connection:

```
$ aws proton accept-environment-account-connection \
    --id "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111"
```

Response:

```
{
    "environmentAccountConnection": {
        "arn": "arn:aws:proton:region-id:123456789222:environment-account-connection/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "environmentAccountId": "123456789222",
        "environmentName": "simple-env-connected",
        "id": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "lastModifiedAt": "2021-04-28T23:15:33.486000+00:00",
        "managementAccountId": "123456789111",
        "requestedAt": "2021-04-28T23:13:50.847000+00:00",
        "roleArn": "arn:aws:iam::123456789222:role/service-role/env-account-proton-service-role",
        "status": "CONNECTED"
    }
}
```

Run the following command to reject the environment account connection:

```
$ aws proton reject-environment-account-connection \
    --id "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111"
```

Response:

```
{
    "environmentAccountConnection": {
        "arn": "arn:aws:proton:us-east-1:123456789222:environment-account-connection/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "status": "REJECTED",
        "environmentAccountId": "123456789222",
        "environmentName": "simple-env-reject",
        "id": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "lastModifiedAt": "2021-04-28T23:13:50.847000+00:00",
        "managementAccountId": "123456789111",
        "requestedAt": "2021-04-28T23:13:50.847000+00:00",
        "roleArn": "arn:aws:iam::123456789222:role/service-role/env-account-proton-service-role"
    }
}
```

**View an environment account connections\. You can *get* or *list* environment account connections**\.

Run the following get command:

```
$ aws proton get-environment-account-connection \
    --id "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111"
```

Response:

```
{
    "environmentAccountConnection": {
        "arn": "arn:aws:proton:region-id:123456789222:environment-account-connection/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "environmentAccountId": "123456789222",
        "environmentName": "simple-env-connected",
        "id": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "lastModifiedAt": "2021-04-28T23:15:33.486000+00:00",
        "managementAccountId": "123456789111",
        "requestedAt": "2021-04-28T23:13:50.847000+00:00",
        "roleArn": "arn:aws:iam::123456789222:role/service-role/env-account-proton-service-role",
        "status": "CONNECTED"
    }
}
```

**Delete an environment account connection in an environment account\.**

**Note**  
If you delete this environment account connection, AWS Proton *won’t* be able to manage the environment infrastructure resources in the environment account until a new environment connection has been accepted for the environment account and named environment\. You're responsible for cleaning up provisioned resources that remain without an environment connection\.

Run the following command:

```
$ aws proton delete-environment-account-connection \
    --id "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111"
```

Response:

```
{
    "environmentAccountConnection": {
        "arn": "arn:aws:proton:us-east-1:123456789222:environment-account-connection/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "environmentAccountId": "123456789222",
        "environmentName": "simple-env-connected",
        "id": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "lastModifiedAt": "2021-04-28T23:13:50.847000+00:00",
        "managementAccountId": "123456789111",
        "requestedAt": "2021-04-28T23:13:50.847000+00:00",
        "roleArn": "arn:aws:iam::123456789222:role/service-role/env-account-proton-service-role",
        "status": "CONNECTED"
    }
}
```

------