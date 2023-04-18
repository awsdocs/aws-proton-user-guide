# Create a service sync configuration<a name="create-service-sync"></a>



You can create a service sync configuration using the console or AWS CLI\.

------
#### [ AWS Management Console ]

1. On the **Choose a service template** page, select a template and choose **Configure**\.

1. On the **Configure service** page, in the **Service details** section, enter a new **Service name**\.

1. \(Optional\) Enter a description for the service\.

1. In the **Application source code repository** section, choose **Choose a linked Git repository** to select a repository you've already linked with AWS Proton\. If you don't already have a linked repository, choose **Link another Git repository** and follow the instructions in [Create a link to your repository](https://docs.aws.amazon.com/proton/latest/userguide/ag-create-repo.html)\.

1. For **Repository**, choose the name of your source code repository from the list\.

1. For **Branch**, choose the name of the repository branch for your source code from the list\.

1. \(Optional\) In the **Tags** section, choose **Add new tag** and enter a key and value to create a customer managed tag\.

1. Choose **Next**\.

1. On the **Configure service instances** page, in the **Service definition source** section, select **Sync your service from Git**\.

1. In the **Service definition files** section, if you want AWS Proton to create your `proton-ops` file, select **I want AWS Proton to create the files**\. With this option, AWS Proton creates the `spec` and `proton-ops` file in the locations you specify\. Select **I am providing my own files** to create your own OPS file\.

1. In the **Service definition repository** section, choose **Choose a linked Git repository** to select a repository you've already linked with AWS Proton\.

1. For **Repository name**, choose the name of your source code repository from the list\.

1. For **`proton-ops` file branch**, choose the name of your branch from the list where AWS Proton will put your OPS and spec file\.

1. In the **Service instances** section, each field is automatically filled based on the values in the `proton-ops` file\.

1. Choose **Next** and review your inputs\.

1. Choose **Create**\.

------
#### [ AWS CLI ]

**Create a service sync configuration using the AWS CLI**
+ Run the following command\.

  ```
  $ aws proton create-service-sync-config \
      --resource "service-arn" \
      --repository-provider "GITHUB" \
      --repository "example/proton-sync-service" \
      --ops-file-branch "main" \
      --proton-ops-file "./configuration/custom-proton-ops.yaml" (optional)
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