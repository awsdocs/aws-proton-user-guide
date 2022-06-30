# Delete a template sync configuration<a name="delete-template-sync"></a>

Delete a template sync configuration using the console or CLI\.

------
#### [ AWS Management Console ]

**Delete a template sync configuration using the console\.**

1. In the template details page, choose the **Sync** tab\.

1. In the **Sync details** section, choose **Disconnect**\.

------
#### [ AWS CLI ]

**The following example commands and responses show how to use the AWS CLI to delete synced template configurations\.**

Run the following command\.

```
$ aws proton delete-template-sync-config \
    --template-name "env-template" \
    --template-type "ENVIRONMENT"
```

The response is as follows\.

```
{
    "templateSyncConfig": {
        "templateName": "env-template",
        "templateType": "ENVIRONMENT"
    }
}
```

------