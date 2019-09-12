# Granting Cross\-Account Access<a name="cross-account-access"></a>

There are two ways in AWS to grant cross\-account access to a resource:
+ Use a resource policy
+ Use an IAM role

**To use a resource policy to grant cross\-account access**

1. An administrator \(or other authorized identity\) in Account A attaches a resource policy to the Data Catalog in Account A\. This policy grants Account B specific cross\-account permissions to perform operations on a resource in Account A's catalog\.

1. An administrator in Account B attaches an IAM policy to a user or other IAM identity in Account B that delegates the permissions received from Account A\.

1. The user or other identity in Account B now has access to the specified resource in Account A\.

   The user needs permission from *both* the resource owner \(Account A\) *and* their parent account \(Account B\) to be able to access the resource\.

**To use an IAM role to grant cross\-account access**

1. An administrator \(or other authorized identity\) in the account that owns the resource \(Account A\) creates an IAM role\.

1. The administrator in Account A attaches a policy to the role that grants cross\-account permissions for access to the resource in question\.

1. The administrator in Account A attaches a trust policy to the role that identifies an IAM identity in a different account \(Account B\) as the principal who can assume the role\.

   The principal in the trust policy can also be an AWS service principal if you want to grant an AWS service permission to assume the role\.

1. An administrator in Account B now delegates permissions to one or more IAM identities in Account B so that they can assume that role\. Doing so gives those identities in Account B access to the resource in account A\.

   For more information about using IAM to delegate permissions, see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\. For more information about users, groups, roles, and permissions, see [Identities \(Users, Groups, and Roles\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the *IAM User Guide*\.

For a comparison of these two approaches, see [How IAM Roles Differ from Resource\-based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html) in the *IAM User Guide*\. AWS Glue supports both options, with the restriction that a resource policy can grant access only to Data Catalog resources\.

For example, to give user Bob in Account B access to database `db1` in Account A, attach the following resource policy to the catalog in Account A\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "glue:GetDatabase"
      ],
      "Principal": {"AWS": [
        "arn:aws:iam::account-B-id:user/Bob"
      ]},
      "Resource": [
        "arn:aws:glue:us-east-1:account-A-id:catalog",      
        "arn:aws:glue:us-east-1:account-A-id:database/db1"
      ]
    }
  ]
}
```

In addition, Account B would have to attach the following IAM policy to Bob before he would actually get access to `db1` in Account A\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "glue:GetDatabase"
      ],
      "Resource": [
        "arn:aws:glue:us-east-1:account-A-id:catalog",      
        "arn:aws:glue:us-east-1:account-A-id:database/db1"
      ]
    }
  ]
}
```

## Making a Cross\-Account API Call<a name="cross-account-calling"></a>

All AWS Glue Data Catalog operations have a `CatalogId` field\. If the required permissions have been granted to enable cross\-account access, a caller can make Data Catalog API calls across accounts\. The caller does this by passing the target AWS account ID in `CatalogId` so as to access the resource in that target account\.

If no `CatalogId` value is provided, AWS Glue uses the caller's own account ID by default, and the call is not cross\-account\.

## Making a Cross\-Account ETL Call<a name="cross-account-calling-etl"></a>

Some AWS Glue PySpark and Scala APIs have a catalog ID field\. If all the required permissions have been granted to enable cross\-account access, an ETL job can make PySpark and Scala calls to API operations across accounts by passing the target AWS account ID in the catalog ID field to access Data Catalog resources in a target account\.

If no catalog ID value is provided, AWS Glue uses the caller's own account ID by default, and the call is not cross\-account\.

For PySpark APIs that support `catalog_id`, see [GlueContext Class](aws-glue-api-crawler-pyspark-extensions-glue-context.md)\. For Scala APIs that support `catalogId`, see [AWS Glue Scala GlueContext APIs](glue-etl-scala-apis-glue-gluecontext.md)\.

The following example shows the permissions required by the grantee to run an ETL job\. In this example, *grantee\-account\-id* is the `catalog-id` of the client running the job and *grantor\-account\-id* is the owner of the resource\. This example grants permission to all catalog resources in the grantor's account\. To limit the scope of resources granted, you can provide specific ARNs for the catalog, database, table, and connection\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "glue:GetConnection",
                "glue:GetDatabase",
                "glue:GetTable",
                "glue:GetPartition" 
            ],
            "Principal": {"AWS": ["arn:aws:iam:grantee-account-id:root"]},
            "Resource": [
                "arn:aws:glue:us-east-1:grantor-account-id:*"
            ]
        }
    ]
}
```

**Note**  
If a table in the grantor's account points to an Amazon S3 location that is also in the grantor's account, the IAM role used to run an ETL job in the grantee's account must have permission to list and get objects from the grantor's account\.

Given that the client in Account A already has permission to create and run ETL jobs, the following are the basic steps to set up an ETL job for cross\-account access:

1. Allow cross\-account data access \(skip this step if Amazon S3 cross\-account access is already set up\)\.

   1. Update the Amazon S3 bucket policy in Account B to allow cross\-account access from Account A\.

   1. Update the IAM policy in Account A to allow access to the bucket in Account B\.

1. Allow cross\-account Data Catalog access\.

   1. Create or update the resource policy attached to the Data Catalog in Account B to allow access from Account A\.

   1. Update the IAM policy in Account A to allow access to the Data Catalog in Account B\.

## AWS Glue Resource Ownership and Operations<a name="access-control-resource-ownership"></a>

Your AWS account owns the AWS Glue Data Catalog resources that are created in that account, regardless of who created them\. Specifically, the owner of a resource is the AWS account of the [principal entity](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) \(that is, the AWS account root user, the IAM user, or the IAM role\) that *authenticated* the creation request for that resource; for example:
+ If you use your AWS account root user credentials to create a table in your Data Catalog, your AWS account is the owner of the resource\.
+ If you create an IAM user in your AWS account and grant permissions to that user to create a table, every table that the user creates is owned by your AWS account, to which the user belongs\.
+ If you create an IAM role in your AWS account with permissions to create a table, anyone who can assume the role can create a table\. But again, your AWS account owns the table resources that are created using that role\.

For each AWS Glue resource, the service defines a set of API operations that apply to it\. To grant permissions for these API operations, AWS Glue defines a set of actions that you can specify in a policy\. Some API operations can require permissions for more than one action in order to perform the API operation\.

## Cross\-Account Resource Ownership and Billing<a name="cross-account-ownership-and-billing"></a>

When a user in one AWS account \(Account A\) creates a new resource such as a database in a different account \(Account B\), that resource is then owned by Account B, the account where it was created\. An administrator in Account B automatically gets full permissions to access the new resource, including reading, writing, and granting access permissions to a third account\. The user in Account A can access the resource that they just created only if they have the appropriate permissions granted by Account B\.

Storage costs and other costs that are directly associated with the new resource are billed to Account B, the resource owner\. The cost of requests from the user who created the resource are billed to the requester's account, Account A\.

 For more information about AWS Glue billing and pricing, see [How AWS Pricing Works](https://d0.awsstatic.com/whitepapers/aws_pricing_overview.pdf)\.

## Cross\-Account Access Limitations<a name="cross-account-limitations"></a>

AWS Glue cross\-account access has the following limitations:
+ Cross\-account access to AWS Glue is not allowed if the resource owner account has not migrated the Amazon Athena data catalog to AWS Glue\. You can find the current migration status using the [GetCatalogImportStatus \(get\_catalog\_import\_status\)](aws-glue-api-catalog-migration.md#aws-glue-api-catalog-migration-GetCatalogImportStatus)\. For more details on how to migrate an Athena catalog to AWS Glue, see [Upgrading to the AWS Glue Data Catalog Step\-by\-Step](https://docs.aws.amazon.com/athena/latest/ug/glue-upgrade.html) in the *Amazon Athena User Guide*\.
+ Cross\-account access is *only* supported for Data Catalog resources, including databases, tables, user\-defined functions, and connections\.
+ Cross\-account access to the Data Catalog is not supported when using an AWS Glue crawler, or Amazon Athena\.