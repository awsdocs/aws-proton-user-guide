# Delete a service sync configuration<a name="delete-service-sync"></a>

You can delete a service sync configuration using the console or AWS CLI\.

------
#### [ AWS Management Console ]

**Delete a service sync configuration using the console**

1. On the service details page, choose the **Service sync** tab\.

1. In the **Service sync details** section, choose **Disconnect** to disconnect your repository\. After your repository is disconnected, we no longer sync the service from that repository\.

------
#### [ AWS CLI ]

**The following example commands and responses show how to use the AWS CLI to delete service synced configurations\.**

Run the following command\.

```
$ aws proton delete-service-sync-config \
    --service-name "service name"
```

The response is as follows\.

```
{
    "serviceSyncConfig": {
        "branch": "main",
        "filePath": "./configuration/custom-proton-ops.yaml",
        "repositoryName": "example/proton-sync-service",
        "repositoryProvider": "GITHUB",
        "serviceName": "service name"
    }
}
```

**Note**  
Service sync doesn't delete service instances\. It only deletes the configuration\.

------