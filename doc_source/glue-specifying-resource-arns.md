# Specifying AWS Glue Resource ARNs<a name="glue-specifying-resource-arns"></a>

In AWS Glue, you can control access to resources using an AWS Identity and Access Management \(IAM\) policy\. In a policy, you use an Amazon Resource Name \(ARN\) to identify the resource that the policy applies to\. Not all resources in AWS Glue support ARNs\.

**Topics**
+ [Data Catalog ARNs](#data-catalog-resource-arns)
+ [ARNs for Non\-Catalog Objects](#non-catalog-resource-arns)
+ [Access Control for AWS Glue Non\-Catalog Singular API Operations](#non-catalog-singular-apis)
+ [Access Control for AWS Glue Non\-Catalog API Operations That Retrieve Multiple Items](#non-catalog-plural-apis)
+ [Access Control for AWS Glue Non\-Catalog Batch Get API Operations](#non-catalog-batch-get-apis)

## Data Catalog ARNs<a name="data-catalog-resource-arns"></a>

Data Catalog resources have a hierarchical structure, with `catalog` as the root\.

```
arn:aws:glue:region:account-id:catalog
```

Each AWS account has a single Data Catalog in an AWS Region with the 12\-digit account ID as the catalog ID\. Resources have unique ARNs associated with them, as shown in the following table\.


| **Resource Type**  |  **ARN Format**  | 
| --- | --- | 
| Catalog |  `arn:aws:glue:region:account-id:catalog` For example: `arn:aws:glue:us-east-1:123456789012:catalog`  | 
| Database |  `arn:aws:glue:region:account-id:database/database name` For example: `arn:aws:glue:us-east-1:123456789012:database/db1`  | 
| Table |  `arn:aws:glue:region:account-id:table/database name/table name` For example: `arn:aws:glue:us-east-1:123456789012:table/db1/tbl1`  | 
| User\-defined function |  `arn:aws:glue:region:account-id:userDefinedFunction/database name/user-defined function name` For example: `arn:aws:glue:us-east-1:123456789012:userDefinedFunction/db1/func1`  | 
| Connection |  `arn:aws:glue:region:account-id:connection/connection name` For example: `arn:aws:glue:us-east-1:123456789012:connection/connection1`  | 

To enable fine\-grained access control, you can use these ARNs in your IAM policies and resource policies to grant and deny access to specific resources\. Wildcards are allowed in the policies\. For example, the following ARN matches all tables in database `default`\.

```
arn:aws:glue:us-east-1:123456789012:table/default/*
```

**Important**  
All operations performed on a Data Catalog resource require permission on the resource and all the ancestors of that resource\. For example, to create a partition for a table requires permission on the table, database, and catalog where the table is located\. The following example shows the permission required to create partitions on table `PrivateTable` in database `PrivateDatabase` in the Data Catalog\.  

```
{
   "Sid": "GrantCreatePartitions",
   "Effect": "Allow",
   "Action": [
       "glue:BatchCreatePartitions"
   ],
   "Resource": [
       "arn:aws:glue:us-east-1:123456789012:table/PrivateDatabase/PrivateTable",
       "arn:aws:glue:us-east-1:123456789012:database/PrivateDatabase",
       "arn:aws:glue:us-east-1:123456789012:catalog"
   ]
}
```
In addition to permission on the resource and all its ancestors, all delete operations require permission on all children of that resource\. For example, deleting a database requires permission on all the tables and user\-defined functions in the database, in addition to the database and the catalog where the database is located\. The following example shows the permission required to delete database `PrivateDatabase` in the Data Catalog\.  

```
{
   "Sid": "GrantDeleteDatabase",
   "Effect": "Allow",
   "Action": [
       "glue:DeleteDatabase"
   ],
   "Resource": [
       "arn:aws:glue:us-east-1:123456789012:table/PrivateDatabase/*",
       "arn:aws:glue:us-east-1:123456789012:userDefinedFunction/PrivateDatabase/*",
       "arn:aws:glue:us-east-1:123456789012:database/PrivateDatabase",
       "arn:aws:glue:us-east-1:123456789012:catalog"
   ]
}
```
In summary, actions on Data Catalog resources follow these permission rules:  
Actions on the catalog require permission on the catalog only\.
Actions on a database require permission on the database and catalog\.
Delete actions on a database require permission on the database and catalog plus all tables and user\-defined functions in the database\.
Actions on a table, partition, or table version require permission on the table, database, and catalog\.
Actions on a user\-defined function require permission on the user\-defined function, database, and catalog\.
Actions on a connection require permission on the connection and catalog\.

## ARNs for Non\-Catalog Objects<a name="non-catalog-resource-arns"></a>

Some AWS Glue resources allow resource\-level permissions to control access using an ARN\. You can use these ARNs in your IAM policies to enable fine\-grained access control\. The following table lists the resources that can contain resource ARNs\.


| **Resource Type**  |  **ARN Format**  | 
| --- | --- | 
| Crawler |  `arn:aws:glue:region:account-id:crawler/crawler-name` For example: `arn:aws:glue:us-east-1:123456789012:crawler/mycrawler`  | 
| Job |  `arn:aws:glue:region:account-id:job/job-name` For example: `arn:aws:glue:us-east-1:123456789012:job/testjob`  | 
| Trigger |  `arn:aws:glue:region:account-id:trigger/trigger-name` For example: `arn:aws:glue:us-east-1:123456789012:trigger/sampletrigger`  | 
| Development endpoint |  `arn:aws:glue:region:account-id:devEndpoint/development-endpoint-name` For example: `arn:aws:glue:us-east-1:123456789012:devEndpoint/temporarydevendpoint`  | 

## Access Control for AWS Glue Non\-Catalog Singular API Operations<a name="non-catalog-singular-apis"></a>

AWS Glue non\-catalog *singular* API operations act on a single item \(development endpoint\)\. Examples are `GetDevEndpoint`, `CreateUpdateDevEndpoint`, and `UpdateDevEndpoint`\. For these operations, a policy must put the API name in the `"action"` block and the resource ARN in the `"resource"` block\.

Suppose that you want to allow a user to call the `GetDevEndpoint` operation\. The following policy grants the minimum necessary permissions to an endpoint named `myDevEndpoint-1`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Minimum permissions",
            "Effect": "Allow",
            "Action": "glue:GetDevEndpoint",
            "Resource": "arn:aws:glue:us-east-1:123456789012:devEndpoint/myDevEndpoint-1"
        }
    ]
}
```

The following policy allows `UpdateDevEndpoint` access to resources that match `myDevEndpoint-` with a wildcard \(\*\)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Permission with wildcard",
            "Effect": "Allow",
            "Action": "glue:UpdateDevEndpoint",
            "Resource": "arn:aws:glue:us-east-1:123456789012:devEndpoint/myDevEndpoint-*"
        }
    ]
}
```

You can combine the two policies as in the following example\. You might see `EntityNotFoundException` for any development endpoint whose name begins with `A`\. However, an access denied error is returned when you try to access other development endpoints\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Combined permissions",
            "Effect": "Allow",
            "Action": [
                "glue:UpdateDevEndpoint",
                "glue:GetDevEndpoint"
            ],
            "Resource": "arn:aws:glue:us-east-1:123456789012:devEndpoint/A*"
        }
    ]
}
```

## Access Control for AWS Glue Non\-Catalog API Operations That Retrieve Multiple Items<a name="non-catalog-plural-apis"></a>

Some AWS Glue API operations retrieve multiple items \(such as multiple development endpoints\); for example, `GetDevEndpoints`\. For this operation, you can specify only a wildcard \(\*\) resource, and not specific ARNs\.

For example, to include `GetDevEndpoints` in the policy, the resource must be scoped to the wildcard \(\*\)\. The singular operations \(`GetDevEndpoint`, `CreateDevEndpoint`, and `DeleteDevendpoint`\) are also scoped to all \(\*\) resources in the example\.

```
{
            "Sid": "Plural API included",
            "Effect": "Allow",
            "Action": [
                "glue:GetDevEndpoints",
                "glue:GetDevEndpoint",
                "glue:CreateDevEndpoint",
                "glue:UpdateDevEndpoint"
            ],
            "Resource": [
                "*"
            ]
}
```

## Access Control for AWS Glue Non\-Catalog Batch Get API Operations<a name="non-catalog-batch-get-apis"></a>

Some AWS Glue API operations retrieve multiple items \(such as multiple development endpoints\); for example, `BatchGetDevEndpoints`\. For this operation, you can specify an ARN to limit the scope of resources that can be accessed\.

For example, to allow access to a specific development endpoint, include `BatchGetDevEndpoints` in the policy with its resource ARN\.

```
{
            "Sid": "BatchGet API included",
            "Effect": "Allow",
            "Action": [
                "glue:BatchGetDevEndpoints"
            ],
            "Resource": [
                "arn:aws:glue:us-east-1:123456789012:devEndpoint/de1" 
            ]
}
```

With this policy, you can successfully access the development endpoint named `de1`\. However, if you try to access the development endpoint named `de2`, an error is returned\.

```
An error occurred (AccessDeniedException) when calling the BatchGetDevEndpoints operation: No access to any requested resource.
```

**Important**  
For alternative approaches to setting up IAM policies, such as using `List` and `BatchGet` API operations, see [AWS Glue Identity\-Based \(IAM\) Access Control Policy with Tags Examples](glue-policy-examples-iam-tags.md)\. 