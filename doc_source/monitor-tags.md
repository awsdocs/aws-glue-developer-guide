# AWS Tags in AWS Glue<a name="monitor-tags"></a>

To help you manage your AWS Glue resources, you can optionally assign your own tags to some AWS Glue resource types\. A tag is a label that you assign to an AWS resource\. Each tag consists of a key and an optional value, both of which you define\. You can use tags in AWS Glue to organize and identify your resources\. Tags can be used to create cost accounting reports and restrict access to resources\. If you're using AWS Identity and Access Management, you can control which users in your AWS account have permission to create, edit, or delete tags\. For more information, see [Identity\-Based Policies \(IAM Policies\) with Tags](using-identity-based-policies.md#glue-identity-based-policy-tags)\.  

In AWS Glue, you can tag the following resources:
+ Crawler
+ Job
+ Trigger
+ Development endpoint

**Note**  
As a best practice, to allow tagging of these AWS Glue resources, always include the `glue:TagResource` action in your policies\.

Consider the following when using tags with AWS Glue\.
+ A maximum of 50 tags are supported per entity\.
+ When you create a tag on an object, **TagKey** is required, **TagValue** is optional\.
+ **TagKey** and **TagValue** are case sensitive\.
+ **TagKey** and **TagValue** must not contain prefix *aws*\. No operations are allowed on such tags\.
+ The maximum **TagKey** length is 128 Unicode characters in UTF\-8\. The **TagKey** must not be empty or null\.
+ The maximum **TagValue** length is 256 Unicode characters in UTF\-8\. The **TagValue** may be empty or null\.

For more information, see [AWS Tagging Strategies](https://aws.amazon.com/answers/account-management/aws-tagging-strategies/)\. 

**Note**  
Currently, AWS Glue does not support the Resource Groups Tagging API\.

For information about how to control access using tags, see [Identity\-Based Policies \(IAM Policies\) with Tags](using-identity-based-policies.md#glue-identity-based-policy-tags)\.