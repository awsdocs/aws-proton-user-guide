# Keep infrastructure up to date with the AWS Proton dashboard<a name="monitoring-dashboard"></a>

The AWS Proton dashboard provides a summary of AWS Proton resources in your AWS account, with a particular focus on *staleness*—how updated deployed resources are\. A deployed resource is up to date when it uses the recommended version of its associated template\. An out\-of\-date deployed resource might need a major or minor template version update\.

## View the dashboard in the AWS Proton console<a name="w133aac32b9b7"></a>

To view the AWS Proton dashboard, open the [AWS Proton console](https://console.aws.amazon.com/proton/), and then, in the navigation pane, choose **Dashboard**\.

![\[The AWS Proton dashboard\]](http://docs.aws.amazon.com/proton/latest/userguide/images/dashboard.png)

The top of the dashboard displays counts of all resources in your account\. Next, the dashboard breaks down resource counts for each deployed resource type by the status of resources of that type\. Finally, a service instance table shows details of each service instance—its deployment status, the AWS Proton resources that it's associated with, the updates that are available to it, and some time stamps\.

You can filter the service instance list by any table property\. For example, you can filter to see service instances with deployments within a specific time window, or service instances that are out of date relative to major or minor version recommendations\.

Choose a service instance name to navigate to the service instance detail page, where you can act to make appropriate version updates\. Choose any other AWS Proton resource name to navigate to its detail page, or choose a resource type to navigate to the respective resource list\.