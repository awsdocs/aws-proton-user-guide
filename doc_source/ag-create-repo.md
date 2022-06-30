# Create and register a link to your repository<a name="ag-create-repo"></a>

You can create and register a link to your repository using the console or CLI\. When you create a repository link, AWS Proton creates a [service linked role](using-service-linked-roles.md) for you\.

------
#### [ AWS Management Console ]

**Create and register a link to your repository as shown in the following console steps\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Repositories**\.

1. Choose **Create repository**\.

1. In the **Link new repository** page, in the **Repository details** section:

   1. Choose your repository provider\.

   1. Choose one of your existing connections\. If you don't have one, choose **Add a new CodeStar connection** to create a connection, and then go back to the AWS Proton console, refresh the connection list, and choose your new connection\.

   1. Choose from your connected source code repositories\.

1. *\[optional\]* In the **Tags** section, choose **Add new tag** one or more times, and enter **Key** and **Value** pairs\.

1. Choose **Create repository**\.

1. View the detail data for your linked repository\.

------
#### [ AWS CLI ]

**Create and register a link to your repository\.**

Run the following command:

```
$ aws proton create-repository \
    --name myrepos/environments \
    --connection-arn "arn:aws:codestar-connections:region-id:123456789012:connection/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111" \
    --provider "GITHUB" \
    --encryption-key "arn:aws:kms:region-id:123456789012:key/bPxRfiCYEXAMPLEKEY \
    --tags key=mytag1,value=value1 key=mytag2,value=value2
```

The last two parameters, \-\-encryption\-key and \-\-tags, are optional\.

Response:

```
{
    "repository": {
        "arn": "arn:aws:proton:region-id:123456789012:repository/github:myrepos/environments",
        "connectionArn": "arn:aws:codestar-connections:region-id:123456789012:connection/2ad03b28-a7c4-EXAMPLE11111",
        "encryptionKey": "arn:aws:kms:region-id:123456789012:key/bPxRfiCYEXAMPLEKEY",
        "name": "myrepos/environments",
        "provider": "GITHUB"
    }
}
```

After you create a repository, you can view a list of AWS and customer managed tags, as shown in the following example command\. AWS Proton automatically generates AWS managed tags for you\. You can also modify and create customer managed tags using the AWS CLI\. For more information, see [AWS Proton resources and tagging](resources.md)\.

Command:

```
$ aws proton list-tags-for-resource \
    --resource-arn "arn:aws:proton:region-id:123456789012:repository/github:myrepos/environments"
```

------