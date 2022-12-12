# Getting started with the AWS CLI<a name="ag-getting-started-cli"></a>

To get started with AWS Proton using the AWS CLI, follow this tutorial\. The tutorial demonstrates a public facing load\-balanced AWS Proton service based on AWS Fargate\. The tutorial also provisions a CI/CD pipeline that deploys a static website with a displayed image\.

Before you start, be sure you are set up correctly\. For details, see [Prerequisites](getting-started-prerequisites.md)\.

## Step 1: Register an environment template<a name="ag-getting-started-cli.env-templ"></a>

In this step, as an administrator, you register an example environment template, which contains an Amazon Elastic Container Service \(Amazon ECS\) cluster and an Amazon Virtual Private Cloud \(Amazon VPC\) with two public/private subnets\.

**To register an environment template**

1. Fork the [AWS Proton Sample CloudFormation Templates](https://github.com/aws-samples/aws-proton-cloudformation-sample-templates/) repository into your GitHub account or organization\. This repository includes the environment and service templates that we use in this tutorial\.

   Then, register your forked repository with AWS Proton\. For more information, see [Create a link to your repository](ag-create-repo.md)\.

1. Create an environment template\.

   The environment template resource tracks environment template versions\.

   ```
   $ aws proton create-environment-template \
     --name "fargate-env" \
     --display-name "Public VPC Fargate" \
     --description "VPC with public access and ECS cluster"
   ```

1. Create a template sync configuration\.

   AWS Proton sets up a sync relationship between your repository and your environment template\. It then creates template version 1\.0 in `DRAFT` status\.

   ```
   $ aws proton create-template-sync-config \
     --template-name "fargate-env" \
     --template-type "ENVIRONMENT" \
     --repository-name "your-forked-repo" \
     --repository-provider "GITHUB" \
     --branch "your-branch" \
     --subdirectory "environment-templates/fargate-env"
   ```

1. Wait for the environment template version to be successfully registered\.

   When this command returns with an exit status of `0`, version registration is complete\. This is useful in scripts to ensure you can successfully run the command in the next step\.

   ```
   $ aws proton wait environment-template-version-registered \
     --template-name "fargate-env" \
     --major-version "1" \
     --minor-version "0"
   ```

1. Publish the environment template version to make it available for environment creation\.

   ```
   $ aws proton update-environment-template-version \
     --template-name "fargate-env" \
     --major-version "1" \
     --minor-version "0" \
     --status "PUBLISHED"
   ```

## Step 2: Register a service template<a name="ag-getting-started-cli.srv-templ"></a>

In this step, as an administrator, you register an example service template, which contains all the resources required to provision an Amazon ECS Fargate service behind a load balancer and a CI/CD pipeline that uses AWS CodePipeline\.

**To register a service template**

1. Create a service template\.

   The service template resource tracks service template versions\.

   ```
   $ aws proton create-service-template \
     --name "load-balanced-fargate-svc" \
     --display-name "Load balanced Fargate service" \
     --description "Fargate service with an application load balancer"
   ```

1. Create a template sync configuration\.

   AWS Proton sets up a sync relationship between your repository and your service template\. It then creates template version 1\.0 in `DRAFT` status\.

   ```
   $ aws proton create-template-sync-config \
     --template-name "load-balanced-fargate-svc" \
     --template-type "SERVICE" \
     --repository-name "your-forked-repo" \
     --repository-provider "GITHUB" \
     --branch "your-branch" \
     --subdirectory "service-templates/load-balanced-fargate-svc"
   ```

1. Wait for the service template version to be successfully registered\.

   When this command returns with an exit status of `0`, version registration is complete\. This is useful in scripts to ensure you can successfully run the command in the next step\.

   ```
   $ aws proton wait service-template-version-registered \
     --template-name "load-balanced-fargate-svc" \
     --major-version "1" \
     --minor-version "0"
   ```

1. Publish the service template version to make it available for service creation\.

   ```
   $ aws proton update-service-template-version \
     --template-name "load-balanced-fargate-svc" \
     --major-version "1" \
     --minor-version "0" \
     --status "PUBLISHED"
   ```

## Step 3: Deploy an environment<a name="ag-getting-started-cli.env-deploy"></a>

In this step, as an administrator, you instantiate an AWS Proton environment from the environment template\.

**To deploy an environment**

1. Get an example spec file for the environment template that you registered\.

   You can download the file `environment-templates/fargate-env/spec/spec.yaml` from the template example repository\. Alternatively, you can fetch the entire repository locally and run the create\-environment command from the `environment-templates/fargate-env` directory\.

1. Create an environment\.

   AWS Proton reads input values from your environment spec, combines them with your environment template, and provisions environment resources in your AWS account using your AWS Proton service role\.

   ```
   $ aws proton create-environment \
     --name "fargate-env-prod" \
     --template-name "fargate-env" \
     --template-major-version 1 \
     --proton-service-role-arn "arn:aws:iam::123456789012:role/AWSProtonServiceRole" \
     --spec "file://spec/spec.yaml"
   ```

1. Wait for the environment to successfully deploy\.

   ```
   $ aws proton wait environment-deployed --name "fargate-env-prod"
   ```

## Step 4: Deploy a service \[application developer\]<a name="ag-getting-started-cli.srv-deploy"></a>

In the previous steps, an administrator registered and published a service template and deployed an environment\. As an application developer, you can now create an AWS Proton service and deploy it into the AWS Proton environment

**To deploy a service**

1. Get an example spec file for the service template that the administrator registered\.

   You can download the file `service-templates/load-balanced-fargate-svc/spec/spec.yaml` from the template example repository\. Alternatively, you can fetch the entire repository locally and run the create\-service command from the `service-templates/load-balanced-fargate-svc` directory\.

1. Fork the [AWS Proton Sample Services](https://github.com/aws-samples/aws-proton-sample-services/) repository into your GitHub account or organization\. This repository includes the application source code that we use in this tutorial\.

1. Create a service\.

   AWS Proton reads input values from your service spec, combines them with your service template, and provisions service resources in your AWS account in the environment that is specified in the spec\. An AWS CodePipeline pipeline deploys your application code from the repository that you specify in the command\.

   ```
   $ aws proton create-service \
     --name "static-website" \
     --repository-connection-arn \
       "arn:aws:codestar-connections:us-east-1:123456789012:connection/your-codestar-connection-id" \
     --repository-id "your-GitHub-account/aws-proton-sample-services" \
     --branch-name "main" \
     --template-major-version 1 \
     --template-name "load-balanced-fargate-svc" \
     --spec "file://spec/spec.yaml"
   ```

1. Wait for the service to successfully deploy\.

   ```
   $ aws proton wait service-created --name "static-website"
   ```

1. Retrieve outputs and view your new website\.

   Run the following command:

   ```
   $ aws proton list-service-instance-outputs \
     --service-name "static-website" \
     --service-instance-name load-balanced-fargate-svc-prod
   ```

   The command's output should be similar to the following:

   ```
   {
       "outputs": [
           {
               "key": "ServiceURL",
               "valueString": "http://your-service-endpoint.us-east-1.elb.amazonaws.com"
           }
       ]
   }
   ```

   The value of the `ServiceURL` instance output is the endpoint to your new service website\. Use your browser to navigate to it\. You should see the following graphic on a static page:  
![\[A web browser showing the static image site resulting from the getting started CLI procedure\]](http://docs.aws.amazon.com/proton/latest/userguide/images/getting-started-static-site.png)

## Step 5: Clean up \(optional\)<a name="ag-getting-started-cli.cleanup"></a>

In this step, when you're done exploring the AWS resources that you created as part of this tutorial, and to save on costs associated with these resources, you delete them\.

**To delete tutorial resources**

1. To delete the service, run the following command:

   ```
   $ aws proton delete-service --name "static-website"
   ```

1. To delete the environment, run the following command:

   ```
   $ aws proton delete-environment --name "fargate-env-prod"
   ```

1. To delete the service template, run the following commands:

   ```
   $ aws proton delete-template-sync-config \
     --template-name "load-balanced-fargate-svc" \
     --template-type "SERVICE"
   $ aws proton delete-service-template --name "load-balanced-fargate-svc"
   ```

1. To delete the environment template, run the following commands:

   ```
   $ aws proton delete-template-sync-config \
     --template-name "fargate-env" \
     --template-type "ENVIRONMENT"
   $ aws proton delete-environment-template --name "fargate-env"
   ```