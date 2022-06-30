# View repository data<a name="ag-repo-view"></a>

You can view and list repository detail data using the console or the AWS CLI\. View repository sync status with the AWS CLI\.

------
#### [ AWS Management Console ]

**List and view repository details using the [AWS Proton console](https://console.aws.amazon.com/proton/)\.**

1. To view a list of your repositories, choose **Repositories** in the navigation pane\.

1. To view detail data, choose the name of a repository\.

   View your repository detail data\.

------
#### [ AWS CLI ]

**View the details of a repository\.**

Run the following command:

```
$ aws proton get-repository \
    --name myrepos/templates \
    --provider "GITHUB"
```

Response:

```
{
    "repository": {
        "arn": "arn:aws:proton:region-id:123456789012:repository/github:myrepos/templates",
        "name": "myrepos/templates",
        "provider": "GITHUB"
    }
}
```

**View a list of your repositories\.**

Run the following command:

```
$ aws proton list-repositories
```

Response:

```
{
    "repositories": [
        {
            "arn": "arn:aws:proton:region-id:123456789012:repository/github:myrepos/templates",
            "name": "myrepos/templates",
            "provider": "GITHUB"
        },
        {
            "arn": "arn:aws:proton:region-id:123456789012:repository/github:myrepos/environments",
            "name": "myrepos/environments",
            "provider": "GITHUB"
        }
    ]
}
```

**View a list of your repository sync definitions\.**

Run the following command:

```
$ aws proton list-repository-sync-definitions \
    --branch "main" \
    --repository-name myrepos/templates \
    --repository-provider "GITHUB" \
    --sync-type "TEMPLATE_SYNC"
```

**View repository sync status for template syncs\.**

Run the following command:

```
$ aws proton get-repository-sync-status \
    --branch "main" \
    --repository-name myrepos/templates \
    --repository-provider "GITHUB" \
    --sync-type "TEMPLATE_SYNC"
```

Response:

```
{
    "latestSync": {
        "events": [
            {
                "event": "Clone started",
                "time": "2021-11-21T00:26:35.883000+00:00",
                "type": "CLONE_STARTED"
            },
            {
                "event": "Updated configuration",
                "time": "2021-11-21T00:26:41.894000+00:00",
                "type": "CONFIG_UPDATED"
            },
            {
                "event": "Starting syncs for commit 62c03ff86eEXAMPLE1111111",
                "externalId": "62c03ff86eEXAMPLE1111111",
                "time": "2021-11-21T00:26:44.861000+00:00",
                "type": "STARTING_SYNC"
            }
        ],
        "startedAt": "2021-11-21T00:26:29.728000+00:00",
        "status": "SUCCEEDED"
    }
}
```

------