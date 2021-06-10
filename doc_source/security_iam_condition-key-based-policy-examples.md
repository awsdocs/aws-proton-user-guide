# Condition\-key based policy examples for AWS Proton<a name="security_iam_condition-key-based-policy-examples"></a>

The following example IAM policy denies access to AWS Proton actions that match the templates specified in the `Condition` block\. Note that these condition keys are only supported by the actions listed at [Actions, resources, and condition keys for AWS Proton](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awsproton.html)\. To manage permissions on other actions, such as `DeleteEnvironmentTemplate`, you must use Resource\-level access control\.

**Example policy that denies AWS Proton template actions on a specific templates:**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": ["proton:*"],
            "Resource": "*",
            "Condition": {
                "StringEqualsIfExists": {
                    "proton:EnvironmentTemplate": ["arn:aws:proton:region_id:123456789012:environment-template/my-environment-template"]
                }
            }
        },
        {
            "Effect": "Deny",
            "Action": ["proton:*"],
            "Resource": "*",
            "Condition": {
                "StringEqualsIfExists": {
                    "proton:ServiceTemplate": ["arn:aws:proton:region_id:123456789012:service-template/my-service-template"]
                }
            }
        }
    ]
}
```

In the next example policy, the first Resource\-level statement denies access to AWS Proton template actions, other than `ListServiceTemplates`, that match the service template listed in the `Resource` block\. The second statement denies access to AWS Proton actions that match the template listed in the `Condition` block\.

**Example policy that denies AWS Proton actions that match a specific template:**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "proton:*"
            ],
            "Resource": "arn:aws:region_id:123456789012:service-template/my-service-template"
        },
        {
            "Effect": "Deny",
            "Action": [
                "proton:*"
            ],
            "Resource": "*",
            "Condition": {
                "StringEqualsIfExists": {
                    "proton:ServiceTemplate": [
                        "arn:aws:proton:region_id:123456789012:service-template/my-service-template"
                    ]
                }
            }
        }
    ]
}
```

The final policy example allows developer AWS Proton actions that match the specific service template listed in the `Condition` block\.

**Example policy to allow AWS Proton developer actions that match a specific template:**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "proton:ListServiceTemplates",
                "proton:ListServiceTemplateVersions",
                "proton:ListServices",
                "proton:ListServiceInstances",
                "proton:ListEnvironments",
                "proton:GetServiceTemplate",
                "proton:GetServiceTemplateVersion",
                "proton:GetService",
                "proton:GetServiceInstance",
                "proton:GetEnvironment",
                "proton:CreateService",
                "proton:UpdateService",
                "proton:UpdateServiceInstance",
                "proton:UpdateServicePipeline",
                "proton:DeleteService",
                "codestar-connections:ListConnections"
            ],
            "Resource": "*",
            "Condition": {
                "StringEqualsIfExists": {
                    "proton:ServiceTemplate": "arn:aws:proton:region_id:123456789012:service-template/my-service-template"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "codestar-connections:PassConnection"
            ],
            "Resource": "arn:aws:codestar-connections:*:*:connection/*",
            "Condition": {
                "StringEquals": {
                    "codestar-connections:PassedToService": "proton.amazonaws.com"
                }
            }
        }

    ]
}
```