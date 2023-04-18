# View configuration details for a service sync<a name="get-service-sync"></a>

You can view the configuration details data for a service sync using the console or AWS CLI\.

------
#### [ AWS Management Console ]

**Use the console to view configuration details for a service sync**

1. In the navigation pane, choose **Services**\.

1. To view detail data, choose the name of a service that you created a service sync configuration for\.

1. In the detail page for the service, select the **Service sync** tab to view the configuration detail data for the service sync\.

------
#### [ AWS CLI ]

**Use the AWS CLI to get a synced service\.**

Run the following command\.

```
$ aws proton get-service-sync-config \
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

**Use the AWS CLI to get the service sync status\.**

Run the following command\.

```
$ aws proton get-service-sync-status \
    --service-name "service name"
```

------