# Service sync configurations<a name="ag-service-sync-configs"></a>

With service sync, you can configure and deploy your AWS Proton services using Git\. You can use service sync to manage initial deployments and updates to your AWS Proton service with a configuration defined in a Git repository\. Through Git, you can use features like version tracking and pull requests to configure, manage, and deploy your services\. Service sync combines AWS Proton and Git to help you provision standardized infrastructure that is defined and managed through AWS Proton templates\. It manages service definitions in your Git repository and reduces tool switching\. Compared to using Git alone, the standardization of templates and deployment in AWS Proton helps you spend less time managing your infrastructure\. AWS Proton also provides higher transparency and auditability for both developers and platform teams\.

## AWS Proton OPS file<a name="service-sync-ops"></a>

The `proton-ops` file defines where AWS Proton finds the spec file that's used to update your service instance\. It also defines what order to update service instances in and when to promote changes from one instance to another\.

The `proton-ops` file supports syncing a service instance using the spec file, or multiple spec files, found in your linked repository\. You can do this by defining a sync block in the `proton-ops` file, like in the following example\.

**Example \./configuration/proton\-ops\.yaml:**

```
sync:
  services:
      frontend-svc:
          alpha:
              branch: dev
              spec: ./frontend-svc/test/frontend-spec.yaml
          beta:
              branch: dev
              spec: ./frontend-svc/test/frontend-spec.yaml
          gamma:
              branch: pre-prod
              spec: ./frontend-svc/pre-prod/frontend-spec.yaml
          prod-one:
              branch: prod
              spec: ./frontend-svc/prod/frontend-spec-second.yaml
          prod-two:
              branch: prod
              spec: ./frontend-svc/prod/frontend-spec-second.yaml
          prod-three:
              branch: prod
              spec: ./frontend-svc/prod/frontend-spec-second.yaml
```

In the preceding example, `frontend-svc` is the service name, and `alpha`, `beta`, `gamma`, `prod-iad`, `prod-cmh`, and `prod-dub` are the instances\.

The `spec` file can be all of the instances or a subset of the instances that are defined within the `proton-ops` file\. However, at minimum, it must have the instance defined within the branch and the spec it's syncing from\. If instances aren't defined in the `proton-ops` file, with the specific branch and `spec` file location, service sync won't create or update those instances\.

The following examples show what the `spec` files look like\. Remember, the `proton-ops` file is synced from these `spec` files\.

**Example `./frontend-svc/test/frontend-spec.yaml`:**

```
proton: "ServiceSpec"
instances:
- name: "alpha"
  environment: "frontend-env"
  spec:
    port: 80
    desired_count: 1
    task_size: "x-small"
    image: "public.ecr.aws/z9d2n7e1/nginx:1.21.0"
- name: "beta"
  environment: "frontend-env"
  spec:
    port: 80
    desired_count: 1
    task_size: "x-small"
    image: "public.ecr.aws/z9d2n7e1/nginx:1.21.0"
```

**Example `./frontend-svc/pre-prod/frontend-spec.yaml`:**

```
proton: "ServiceSpec"
instances:
- name: "gamma"
  environment: "frontend-env"
  spec:
    port: 80
    desired_count: 1
    task_size: "x-small"
    image: "public.ecr.aws/z9d2n7e1/nginx:1.21.0"
```

**Example `./frontend-svc/prod/frontend-spec-second.yaml`:**

```
proton: "ServiceSpec"
instances:
- name: "prod-one"
  environment: "frontend-env"
  spec:
    port: 80
    desired_count: 1
    task_size: "x-small"
    image: "public.ecr.aws/z9d2n7e1/nginx:1.21.0"
- name: "prod-two"
  environment: "frontend-env"
  spec:
    port: 80
    desired_count: 1
    task_size: "x-small"
    image: "public.ecr.aws/z9d2n7e1/nginx:1.21.0"
- name: "prod-three"
  environment: "frontend-env"
  spec:
    port: 80
    desired_count: 1
    task_size: "x-small"
    image: "public.ecr.aws/z9d2n7e1/nginx:1.21.0"
```

If an instance doesn't sync, and there's a continuing issue when trying to sync it, calling the [https://docs.aws.amazon.com/proton/latest/APIReference/API_GetServiceInstanceSyncStatus.html](https://docs.aws.amazon.com/proton/latest/APIReference/API_GetServiceInstanceSyncStatus.html) API may help in resolving the issue\.

**Note**  
Customers using service sync are still restricted by AWS Proton limits\.

**Blockers**

By syncing your service using AWS Proton service sync, you can update your service spec and create and update service instances from your Git repository\. However, there may be moments where you need to update a service or instance manually through the AWS Management Console or AWS CLI\.

AWS Proton helps avoid overwriting any manual changes you make through the AWS Management Console or AWS CLI, such as updating a service instance or deleting a service instance\. To achieve this, AWS Proton automatically creates a service sync blocker by disabling service sync when it detects a manual change\.

To get all the blockers associated with a service, you must do the following in order for each `serviceInstance` associated to the service:
+ Call the `getServiceSyncBlockerSummary` API with only the `serviceName`\.
+ Call the `getServiceSyncBlockerSummary` API with the `serviceName` and `serviceInstanceName`\.

This returns a list of the most recent blockers and the status associated with them\. If any blockers are marked **ACTIVE**, you must resolve them by calling the `UpdateServiceSyncBlocker` API with the `blockerId` and `resolvedReason` for each one\.

If you manually update or create a service instance, AWS Proton creates a service sync blocker on the service instance\. AWS Proton continues to sync all other service instances, but disables the syncing of this service instance until the blocker is resolved\. If you delete a service instance from a service, AWS Proton creates a service sync blocker on the service\. This prevents AWS Proton from syncing any of the service instances until the blocker has been resolved\.

After you have all the active blockers, you must resolve them by calling the `UpdateServiceSyncBlocker` API with the `blockerId` and `resolvedReason` for each of the active blockers\.

Using the AWS Management Console, you can determine if a service sync is disabled by navigating to AWS Proton and choosing the **Service Sync** tab\. If the service or service instances are blocked, an **Enable** button appears\. To enable service sync, choose **Enable**\.

**Topics**
+ [AWS Proton OPS file](#service-sync-ops)
+ [Create a service sync configuration](create-service-sync.md)
+ [View configuration details for a service sync](get-service-sync.md)
+ [Edit a service sync configuration](update-service-sync.md)
+ [Delete a service sync configuration](delete-service-sync.md)