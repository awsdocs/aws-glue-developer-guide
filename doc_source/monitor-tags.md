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
+ In AWS Glue, you specify tags as a list of key\-value pairs in the format `{"string": "string" ...}`
+ When you create a tag on an object, the tag key is required, and the tag value is optional\.
+ The tag key and tag value are case sensitive\.
+ The tag key and the tag value must not contain the prefix *aws*\. No operations are allowed on such tags\.
+ The maximum tag key length is 128 Unicode characters in UTF\-8\. The tag key must not be empty or null\.
+ The maximum tag value length is 256 Unicode characters in UTF\-8\. The tag value may be empty or null\.

## Examples<a name="TagExamples"></a>

The following examples create a job with assigned tags\. 

**AWS CLI**

```
aws glue create-job --name job-test-tags --role MyJobRole --command Name=glueetl,ScriptLocation=S3://aws-glue-scripts//prod-job1 --tags '{"key1" : "value1", "key2 : "value2"}' 
```

**AWS CloudFormation JSON**

```
{
  "Description": "AWS Glue Job Test Tags",
  "Resources": {
    "MyJobRole": {
      "Type": "AWS::IAM::Role",
      "Properties": {
        "AssumeRolePolicyDocument": {
          "Version": "2012-10-17",
          "Statement": [
            {
              "Effect": "Allow",
              "Principal": {
                "Service": [
                  "glue.amazonaws.com"
                ]
              },
              "Action": [
                "sts:AssumeRole"
              ]
            }
          ]
        },
        "Path": "/",
        "Policies": [
          {
            "PolicyName": "root",
            "PolicyDocument": {
              "Version": "2012-10-17",
              "Statement": [
                {
                  "Effect": "Allow",
                  "Action": "*",
                  "Resource": "*"
                }
              ]
            }
          }
        ]
      }
    },
    "MyJob": {
      "Type": "AWS::Glue::Job",
      "Properties": {
        "Command": {
          "Name": "glueetl",
          "ScriptLocation": "s3://aws-glue-scripts//prod-job1"
        },
        "DefaultArguments": {
          "--job-bookmark-option": "job-bookmark-enable"
        },
        "ExecutionProperty": {
          "MaxConcurrentRuns": 2
        },
        "MaxRetries": 0,
        "Name": "cf-job1",
        "Role": {
          "Ref": "MyJobRole",
		"Tags": {
          "key1": "value1", "key2":"value2"
        } 
        }
      }
    }
  }
}
```

For more information, see [AWS Tagging Strategies](https://aws.amazon.com/answers/account-management/aws-tagging-strategies/)\. 

**Note**  
Currently, AWS Glue does not support the Resource Groups Tagging API\.

For information about how to control access using tags, see [Identity\-Based Policies \(IAM Policies\) with Tags](using-identity-based-policies.md#glue-identity-based-policy-tags)\.