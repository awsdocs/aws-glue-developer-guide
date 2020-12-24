# Crawler Prerequisites<a name="crawler-prereqs"></a>

The AWS Identity and Access Management \(IAM\) role that you specify for a crawler must have permission to access the data store that is crawled, and permission to create and update tables and partitions in the AWS Glue Data Catalog\.

For your crawler, you can create a role and attach the following policies:
+ The `AWSGlueServiceRole` AWS managed policy, which grants the required permissions on the Data Catalog
+ An inline policy that grants permissions on the data source\.

A quicker approach is to let the AWS Glue console crawler wizard create a role for you\. The role that it creates is specifically for the crawler, and includes the `AWSGlueServiceRole` AWS managed policy plus the required inline policy for the specified data source\.

If you specify an existing role for a crawler, ensure that it includes the `AWSGlueServiceRole` policy or equivalent \(or a scoped down version of this policy\), plus the required inline policies\. For example, for an Amazon S3 data store, the inline policy would at a minimum be the following: 

```
{
   "Version": "2012-10-17",
    "Statement": [
        {
          "Effect": "Allow",
          "Action": [
              "s3:GetObject",
              "s3:PutObject"
          ],
          "Resource": [
              "arn:aws:s3:::bucket/object*"
          ]
        }
    ]
}
```

For an Amazon DynamoDB data store, the policy would at a minimum be the following: 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:DescribeTable",
        "dynamodb:Scan"
      ],
      "Resource": [
        "arn:aws:dynamodb:region:account-id:table/table-name*"
      ]
    }
  ]
}
```

In addition, if the crawler reads AWS Key Management Service \(AWS KMS\) encrypted Amazon S3 data, then the IAM role must have decrypt permission on the AWS KMS key\. For more information, see [Step 2: Create an IAM Role for AWS Glue](create-an-iam-role.md)\.