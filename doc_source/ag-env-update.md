# Update an environment<a name="ag-env-update"></a>

If the AWS Proton environment is associated with an environment account connection, *don't* update or include the `protonServiceRoleArn` parameter to update or connect to an environment account connection\.

You can only update to a new environment account connection if both of the following is true:
+ It was created in the same environment account that the current environment account connection was created in\.
+ It is associated with the current environment\.

If the environment *isn’t* associated with an environment account connection, *don’t* update or include the `environmentAccountConnectionId` parameter\.

You can update either the `environmentAccountConnectionId` or `protonServiceRoleArn` parameter and value\. You can’t update both\.

If your environment uses self\-managed provisioning, *don't* update the `provisioning-repository` parameter and *omit* the `environmentAccountConnectionId` and `protonServiceRoleArn` parameters\.

There are four modes for updating an environment as described in the following list\. When using the AWS CLI, the `deployment-type` field defines the mode\. When using the console, these modes map to the **Edit**, **Update**, **Update minor**, and **Update major** actions that drop down from **Actions**\.

  
`NONE`  
In this mode, a deployment *doesn't* occur\. Only the requested metadata parameters are updated\.

  
`CURRENT_VERSION`  
In this mode, the environment is deployed and updated with the new spec that you provide\. Only requested parameters are updated\. *Don’t* include minor or major version parameters when you use this `deployment-type`\.

  
`MINOR_VERSION`  
In this mode, the environment is deployed and updated with the published, recommended \(latest\) minor version of the current major version in use by default\. You can also specify a different minor version of the current major version in use\.

  
`MAJOR_VERSION`  
In this mode, the environment is deployed and updated with the published, recommended \(latest\) major and minor version of the current template by default\. You can also specify a different major version that is higher than the major version in use and a minor version \(optional\)\.

**Topics**
+ [Update an AWS\-managed provisioning environment](#ag-env-std-update)
+ [Update a self\-managed provisioning environment](#ag-env-pr-update)
+ [Cancel an environment deployment in progress](#ag-env-cancel)

## Update an AWS\-managed provisioning environment<a name="ag-env-std-update"></a>

Standard provisioning is only supported by environments that provision with AWS CloudFormation\.

**Use the console or AWS CLI to update your environment\.**

------
#### [ AWS Management Console ]

**Update an environment using the console as shown in the following steps\.**

1. 

**Choose 1 of the following 2 steps\.**

   1. 

**In the list of environments\.**

      1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environments**\.

      1. In the list of environments, choose the radio button to the left of the environment that you want to update\.

   1. 

****In the console environment detail page\.****

      1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environments**\.

      1. In the list of environments, choose the name of the environment that you want to update\.

1. 

**Choose 1 of the next 4 steps to update your environment\.**

   1. 

**To make an edit that doesn't require environment deployment\.**

      1. For example, to change a description\.

         Choose **Edit**\.

      1. Fill out the form and choose **Next**\.

      1. Review your edit and choose **Update**\.

   1. 

**To make updates to metadata inputs only\.**

      1. Choose **Actions** and then **Update**\.

      1. Fill out the form and choose **Edit**\.

      1. Fill out the forms and choose **Next** until you reach the **Review** page\.

      1. Review your updates and choose **Update**\.

   1. 

**To make an update to a new minor version of its environment template\.**

      1. Choose **Actions** and then **Update minor**\.

      1. Fill out the form and choose **Next**\.

      1. Fill out the forms and choose **Next** until you reach the **Review** page\.

      1. Review your updates and choose **Update**\.

   1. 

**To make an update to a new major version of its environment template\.**

      1. Choose **Actions** and then **Update major**\.

      1. Fill out the form and choose **Next**\.

      1. Fill out the forms and choose **Next** until you reach the **Review** page\.

      1. Review your updates and choose **Update**\.

------
#### [ AWS CLI ]

**Use the AWS Proton AWS CLI to update an environment to a new minor version\.**

Run the following command to update your environment:

```
$ aws proton update-environment \
        --name "MySimpleEnv" \
        --deployment-type "MINOR_VERSION" \
        --template-major-version "1" \
        --template-minor-version "1" \
        --proton-service-role-arn arn:aws:iam::123456789012:role/service-role/ProtonServiceRole \
        --spec "file:///spec.yaml"
```

Response:

```
{
    "environment": {
        "arn": "arn:aws:proton:region-id:123456789012:environment/MySimpleEnv",
        "createdAt": "2021-04-02T17:29:55.472000+00:00",
        "deploymentStatus": "IN_PROGRESS",        
        "lastDeploymentAttemptedAt": "2021-04-02T17:48:26.307000+00:00",
        "lastDeploymentSucceededAt": "2021-04-02T17:29:55.472000+00:00",
        "name": "MySimpleEnv",
        "protonServiceRoleArn": "arn:aws:iam::123456789012:role/service-role/ProtonServiceRole",
        "templateMajorVersion": "1",
        "templateMinorVersion": "0",
        "templateName": "simple-env"
    }
}
```

Run the following command to get and confirm the status:

```
$ aws proton get-environment \
        --name "MySimpleEnv"
```

Response:

```
{
    "environment": {
        "arn": "arn:aws:proton:region-id:123456789012:environment/MySimpleEnv",
        "createdAt": "2021-04-02T17:29:55.472000+00:00",
        "deploymentStatus": "SUCCEEDED",
        "environmentName": "MySimpleEnv",
        "lastDeploymentAttemptedAt": "2021-04-02T17:48:26.307000+00:00",
        "lastDeploymentSucceededAt": "2021-04-02T17:48:26.307000+00:00",
        "protonServiceRoleArn": "arn:aws:iam::123456789012:role/service-role/ProtonServiceRole",
        "spec": "proton: EnvironmentSpec\n\nspec:\n  my_sample_input: hello\n  my_other_sample_input: everybody\n",
        "templateMajorVersion": "1",
        "templateMinorVersion": "1",
        "templateName": "simple-env"
    }
}
```

------

## Update a self\-managed provisioning environment<a name="ag-env-pr-update"></a>

Self\-managed provisioning is only supported by environments that provision with Terraform\.

**Use the console or AWS CLI to update your environment\.**

------
#### [ AWS Management Console ]

**Update an environment using the console as shown in the following steps\.**

1. 

**Choose 1 of the following 2 steps\.**

   1. 

**In the list of environments\.**

      1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environments**\.

      1. In the list of environments, choose the radio button to the left of the environment template that you want to update\.

   1. 

****In the console environment detail page\.****

      1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environments**\.

      1. In the list of environments, choose the name of the environment that you want to update\.

1. 

**Choose 1 of the next 4 steps to update your environment\.**

   1. 

**To make an edit that doesn't require environment deployment\.**

      1. For example, to change a description\.

         Choose **Edit**\.

      1. Fill out the form and choose **Next**\.

      1. Review your edit and choose **Update**\.

   1. 

**To make updates to metadata inputs only\.**

      1. Choose **Actions** and then **Update**\.

      1. Fill out the form and choose **Edit**\.

      1. Fill out the forms and choose **Next** until you reach the **Review** page\.

      1. Review your updates and choose **Update**\.

   1. 

**To make an update to a new minor version of its environment template\.**

      1. Choose **Actions** and then **Update minor**\.

      1. Fill out the form and choose **Next**\.

      1. Fill out the forms and choose **Next** until you reach the **Review** page\.

      1. Review your updates and choose **Update**\.

   1. 

**To make an update to a new major version of its environment template\.**

      1. Choose **Actions** and then **Update major**\.

      1. Fill out the form and choose **Next**\.

      1. Fill out the forms and choose **Next** until you reach the **Review** page\.

      1. Review your updates and choose **Update**\.

------
#### [ AWS CLI ]

**Use the AWS CLI to update a Terraform environment to a new minor version with self\-managed provisioning\.**

1. Run the following command to update your environment:

   ```
   $ aws proton update-environment \
       --name "pr-environment" \
       --deployment-type "MINOR_VERSION" \
       --template-major-version "1" \
       --template-minor-version "1" \
       --provisioning-repository "branch=main,name=myrepos/env-repo,provider=GITHUB" \
       --spec "file://env-spec-mod.yaml"
   ```

   Response:

   ```
   {
       "environment": {
           "arn": "arn:aws:proton:region-id:123456789012:environment/pr-environment",
           "createdAt": "2021-11-18T21:09:15.745000+00:00",
           "deploymentStatus": "IN_PROGRESS",
           "lastDeploymentAttemptedAt": "2021-11-18T21:25:41.998000+00:00",
           "lastDeploymentSucceededAt": "2021-11-18T21:09:15.745000+00:00",
           "name": "pr-environment",
           "provisioningRepository": {
               "arn": "arn:aws:proton:region-id:123456789012:repository/github:myrepos/env-repo",
               "branch": "main",
               "name": "myrepos/env-repo",
               "provider": "GITHUB"
           },
           "templateMajorVersion": "1",
           "templateMinorVersion": "0",
           "templateName": "pr-env-template"
       }
   }
   ```

1. Run the following command to get and confirm the status:

   ```
   $ aws proton get-environment \
       --name "pr-environment"
   ```

   Response:

   ```
   {
       "environment": {
           "arn": "arn:aws:proton:region-id:123456789012:environment/pr-environment",
           "createdAt": "2021-11-18T21:09:15.745000+00:00",
           "deploymentStatus": "SUCCEEDED",
           "lastDeploymentAttemptedAt": "2021-11-18T21:25:41.998000+00:00",
           "lastDeploymentSucceededAt": "2021-11-18T21:25:41.998000+00:00",
           "name": "pr-environment",
           "provisioningRepository": {
               "arn": "arn:aws:proton:region-id:123456789012:repository/github:myrepos/env-repo",
               "branch": "main",
               "name": "myrepos/env-repo",
               "provider": "GITHUB"
           },
           "spec": "proton: EnvironmentSpec\nspec:\n   ssm_parameter_value: \"test\"\n ssm_another_parameter_value: \"update\"\n",
           "templateMajorVersion": "1",
           "templateMinorVersion": "1",
           "templateName": "pr-env-template"
       }
   }
   ```

1. Review the pull request that was sent by AWS Proton\.
   + If you approve the request, provisioning is in progress\.
   + If you reject the request, the environment creation is cancelled\.
   + If the pull request times out, environment creation is not complete\.

1. Provide provisioning status to AWS Proton\.

   ```
   $ aws proton notify-resource-deployment-status-change \
       --resource-arn "arn:aws:proton:region-id:123456789012:environment/pr-environment" \
       --status "SUCCEEDED"
   ```

------

## Cancel an environment deployment in progress<a name="ag-env-cancel"></a>

You can attempt to cancel an environment update deployment if the `deploymentStatus` is in `IN_PROGRESS`\. AWS Proton attempts to cancel the deployment\. Successful cancellation *isn’t* guaranteed\.

When you cancel an update deployment, AWS Proton attempts to cancel the deployment as listed in the following steps\.

**With AWS\-managed provisioning, AWS Proton:**
+ Sets the deployment state to `CANCELLING`\.
+ Stops the deployment in progress and deletes any new resources that were created by the deployment when `IN_PROGRESS`\.
+ Sets the deployment state to `CANCELLED`\.
+ Reverts the state of the resource to what it was before the deployment was started\.

**With self\-managed provisioning, AWS Proton:**
+ Attempts to close the pull request to prevent merging the changes to your repository\.
+ Sets the deployment state to `CANCELLED` if the pull request was successfully closed\.

For instructions on how to cancel an environment deployment, see [CancelEnvironmentDeployment](https://docs.aws.amazon.com/proton/latest/APIReference/API_CancelEnvironmentDeployment.html) in the *AWS Proton API Reference*\.

You can use the console or CLI to cancel environments that are in progress\.

------
#### [ AWS Management Console ]

**Use the console to cancel an environment update deployment as shown in the following steps\.**

1. In the [AWS Proton console](https://console.aws.amazon.com/proton/), choose **Environments** in the navigation pane\.

1. In the list of environments, choose the name of the environment with the deployment update that you want to cancel\.

1. If your update deployment status is **In progress**, in the environment detail page, choose **Actions** and then **Cancel deployment**\.

1. A modal prompts you to confirm the cancellation\. Choose **Cancel deployment**\.

1. Your update deployment status is set to **Cancelling** and then **Cancelled** to complete the cancellation\.

------
#### [ AWS CLI ]

**Use the AWS Proton AWS CLI to cancel an IN\_PROGRESS environment update deployment to a new minor version 2\.**

A wait condition is included in the template used for this example so that the cancellation starts before the update deployment succeeds\.

Run the following command to cancel the update:

```
$ aws proton cancel-environment-deployment \
        --environment-name "MySimpleEnv"
```

Response:

```
{
    "environment": {
        "arn": "arn:aws:proton:region-id:123456789012:environment/MySimpleEnv",
        "createdAt": "2021-04-02T17:29:55.472000+00:00",
        "deploymentStatus": "CANCELLING",
        "lastDeploymentAttemptedAt": "2021-04-02T18:15:10.243000+00:00",
        "lastDeploymentSucceededAt": "2021-04-02T17:48:26.307000+00:00",
        "name": "MySimpleEnv",
        "protonServiceRoleArn": "arn:aws:iam::123456789012:role/service-role/ProtonServiceRole",
        "spec": "proton: EnvironmentSpec\n\nspec:\n  my_sample_input: hello\n  my_other_sample_input: everybody\n",
        "templateMajorVersion": "1",
        "templateMinorVersion": "1",
        "templateName": "simple-env"
    }
}
```

Run the following command to get and confirm the status:

```
$ aws proton get-environment \
        --name "MySimpleEnv"
```

Response:

```
{
    "environment": {
        "arn": "arn:aws:proton:region-id:123456789012:environment/MySimpleEnv",
        "createdAt": "2021-04-02T17:29:55.472000+00:00",
        "deploymentStatus": "CANCELLED",
        "deploymentStatusMessage": "User initiated cancellation.",
        "lastDeploymentAttemptedAt": "2021-04-02T18:15:10.243000+00:00",
        "lastDeploymentSucceededAt": "2021-04-02T17:48:26.307000+00:00",
        "name": "MySimpleEnv",
        "protonServiceRoleArn": "arn:aws:iam::123456789012:role/service-role/ProtonServiceRole",
        "spec": "proton: EnvironmentSpec\n\nspec:\n  my_sample_input: hello\n  my_other_sample_input: everybody\n",
        "templateMajorVersion": "1",
        "templateMinorVersion": "1",
        "templateName": "simple-env"
    }
}
```

------