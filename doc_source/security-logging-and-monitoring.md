--------

**AWS Proton is governed as a preview program under the [AWS Service Terms](https://aws.amazon.com/service-terms/)\. Report issues and feature requests by connecting with us at [GitHub](https://github.com/aws/aws-proton-public-roadmap) where you can open issues, provide feedback and report bugs\.**

## Additional terms and conditions<a name="preview-banner"></a>
+ The Preview is intended only for evaluation with development or test workloads\. You should not use it for production workloads\.
+ AWS may change the functionality of the Preview during and after the term of the Preview at its sole discretion or based on participant feedback\. The functionality, features, and documentation may change during the Preview term and may be different from any generally available version\.
+ Although the Preview is a free service, you are responsible for fees incurred for other AWS Services that you use in connection with the Preview\. Standard pricing will apply for your use of those AWS Services\.

--------

# Logging and monitoring<a name="security-logging-and-monitoring"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of AWS Proton and your other AWS solutions\. AWS provides the following monitoring tools to watch your instances running in Proton, report when something is wrong, and take automatic actions when appropriate\.

At this time, Proton itself is not integrated with Amazon CloudWatch Logs or AWS Trusted Advisor\. Administrators can configure and use CloudWatch to monitor other AWS services as defined in their service and environment templates\. Proton is integrated with AWS CloudTrail\.
+ *Amazon CloudWatch* monitors your AWS resources and the applications you run on AWS in real time\. You can collect and track metrics, create customized dashboards, and set alarms that notify you or take actions when a specified metric reaches a threshold that you specify\. For example, you can have CloudWatch track CPU usage or other metrics of your Amazon EC2 instances and automatically launch new instances when needed\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.
+ *Amazon CloudWatch Logs* enables you to monitor, store, and access your log files from Amazon EC2 instances, CloudTrail, and other sources\. CloudWatch Logs can monitor information in the log files and notify you when certain thresholds are met\. You can also archive your log data in highly durable storage\. For more information, see the [Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)\.
+ *AWS CloudTrail* captures API calls and related events made by or on behalf of your AWS account and delivers the log files to an Amazon S3 bucket that you specify\. You can identify which users and accounts called AWS, the source IP address from which the calls were made, and when the calls occurred\. For more information, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.
+ *Amazon EventBridge* is a serverless event bus service that makes it easy to connect your applications with data from a variety of sources\. EventBridge delivers a stream of real\-time data from your own applications, Software\-as\-a\-Service \(SaaS\) applications, and AWS services and routes that data to targets such as Lambda\. This enables you to monitor events that happen in services, and build event\-driven architectures\. For more information, see the [EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html)\.