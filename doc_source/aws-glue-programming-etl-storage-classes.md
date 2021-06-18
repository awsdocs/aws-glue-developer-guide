# Excluding Amazon S3 Storage Classes<a name="aws-glue-programming-etl-storage-classes"></a>

If you're running AWS Glue ETL jobs that read files or partitions from Amazon Simple Storage Service \(Amazon S3\), you can exclude some Amazon S3 storage class types\.

The following storage classes are available in Amazon S3:
+ `STANDARD` — For general\-purpose storage of frequently accessed data\.
+ `INTELLIGENT_TIERING` — For data with unknown or changing access patterns\.
+ `STANDARD_IA` and `ONEZONE_IA` — For long\-lived, but less frequently accessed data\.
+ `GLACIER`, `DEEP_ARCHIVE`, and `REDUCED_REDUNDANCY` — For long\-term archive and digital preservation\.

For more information, see [Amazon S3 Storage Classes](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html) in the *Amazon S3 Developer Guide*\.

The examples in this section show how to exclude the `GLACIER` and `DEEP_ARCHIVE` storage classes\. These classes allow you to list files, but they won't let you read the files unless they are restored\. \(For more information, see [Restoring Archived Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/restoring-objects.html) in the *Amazon S3 Developer Guide*\.\)

By using storage class exclusions, you can ensure that your AWS Glue jobs will work on tables that have partitions across these storage class tiers\. Without exclusions, jobs that read data from these tiers fail with the following error: AmazonS3Exception: The operation is not valid for the object's storage class\.

There are different ways that you can filter Amazon S3 storage classes in AWS Glue\.

**Topics**
+ [Excluding Amazon S3 Storage Classes When Creating a Dynamic Frame](#aws-glue-programming-etl-storage-classes-dynamic-frame)
+ [Excluding Amazon S3 Storage Classes on a Data Catalog Table](#aws-glue-programming-etl-storage-classes-table)

## Excluding Amazon S3 Storage Classes When Creating a Dynamic Frame<a name="aws-glue-programming-etl-storage-classes-dynamic-frame"></a>

To exclude Amazon S3 storage classes while creating a dynamic frame, use `excludeStorageClasses` in `additionalOptions`\. AWS Glue automatically uses its own Amazon S3 `Lister` implementation to list and exclude files corresponding to the specified storage classes\.

The following Python and Scala examples show how to exclude the `GLACIER` and `DEEP_ARCHIVE` storage classes when creating a dynamic frame\.

Python example:

```
glueContext.create_dynamic_frame.from_catalog(
    database = "my_database",
    tableName = "my_table_name",
    redshift_tmp_dir = "",
    transformation_ctx = "my_transformation_context",
    additional_options = {
        "excludeStorageClasses" : ["GLACIER", "DEEP_ARCHIVE"]
    }
)
```

Scala example:

```
val* *df = glueContext.getCatalogSource(
    nameSpace, tableName, "", "my_transformation_context",  
    additionalOptions = JsonOptions(
        Map("excludeStorageClasses" -> List("GLACIER", "DEEP_ARCHIVE"))
    )
).getDynamicFrame()
```

## Excluding Amazon S3 Storage Classes on a Data Catalog Table<a name="aws-glue-programming-etl-storage-classes-table"></a>

You can specify storage class exclusions to be used by an AWS Glue ETL job as a table parameter in the AWS Glue Data Catalog\. You can include this parameter in the `CreateTable` operation using the AWS Command Line Interface \(AWS CLI\) or programmatically using the API\. For more information, see [Table Structure](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-catalog-tables.html#aws-glue-api-catalog-tables-Table) and [CreateTable](https://docs.aws.amazon.com/glue/latest/webapi/API_CreateTable.html)\. 

You can also specify excluded storage classes on the AWS Glue console\.

**To exclude Amazon S3 storage classes \(console\)**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane on the left, choose **Tables**\.

1. Choose the table name in the list, and then choose **Edit table**\.

1. In **Table properties**, add **excludeStorageClasses** as a key and **\[\\"GLACIER\\",\\"DEEP\_ARCHIVE\\"\]** as a value\.

1. Choose **Apply**\.