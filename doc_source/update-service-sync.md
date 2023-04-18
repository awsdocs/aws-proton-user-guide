# Edit a service sync configuration<a name="update-service-sync"></a>

You can edit a service sync configuration using the console or AWS CLI\.



------
#### [ AWS Management Console ]

Edit a service sync configuration using the console\.

1. In the navigation pane, choose **Services**\.

1. To view detail data, choose the name of a service that you created a service sync configuration for\.

1. On the service detail page, choose the **Service sync** tab\.

1. In the **Service sync** section, choose **Edit**\.

1. On the **Edit** page, update the information you want to edit and then choose **Save**\.

------
#### [ AWS CLI ]

**The following example command and response shows how you can edit a service sync configuration using the AWS CLI\.**

Run the following command\.

```
$ aws proton update-service-sync-config \
    --service-name "service name" \
    --repository-provider "GITHUB" \
    --repository "example/proton-sync-service" \
    --ops-file-branch "main" \
    --ops-file "./configuration/custom-proton-ops.yaml"
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

------