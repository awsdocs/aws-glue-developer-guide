# Moving Data to and from Amazon Redshift<a name="aws-glue-programming-etl-redshift"></a>

When moving data to and from an Amazon Redshift cluster, AWS Glue jobs issue COPY and UNLOAD statements against Amazon Redshift to achieve maximum throughput\. These commands require that the Amazon Redshift cluster access Amazon Simple Storage Service \(Amazon S3\) as a staging directory\. By default, AWS Glue passes in temporary credentials that are created using the role that you specified to run the job\. For security purposes, these credentials expire after 1 hour, which can cause long running jobs to fail\.

To address this issue, you can associate one or more IAM roles with the Amazon Redshift cluster itself\. COPY and UNLOAD can use the role, and Amazon Redshift refreshes the credentials as needed\. For more information about associating a role with your Amazon Redshift cluster, see [IAM Permissions for COPY, UNLOAD, and CREATE LIBRARY](https://docs.aws.amazon.com/redshift/latest/dg/copy-usage_notes-access-permissions.html#copy-usage_notes-iam-permissions) in the *Amazon Redshift Database Developer Guide*\.  Make sure that the role you associate with your cluster has permissions to read from and write to the Amazon S3 temporary directory that you specified in your job\.

After you set up a role for the cluster, you need to specify it in ETL \(extract, transform, and load\) statements in the AWS Glue script\. The syntax depends on how your script reads and writes your dynamic frame\. If your script reads from an AWS Glue Data Catalog table, you can specify a role as follows:

```
 glueContext.create_dynamic_frame.from_catalog(
    database = "database-name", 
    table_name = "table-name", 
    redshift_tmp_dir = args["TempDir"], 
    additional_options = {"aws_iam_role": "arn:aws:iam::account-id:role/role-name"})
```

Similarly, if your scripts writes a dynamic frame and reads from an Data Catalog, you can specify the role as follows:

```
 glueContext.write_dynamic_frame.from_catalog(
    database = "database-name", 
    table_name = "table-name", 
    redshift_tmp_dir = args["TempDir"], 
    additional_options = {"aws_iam_role": "arn:aws:iam::account-id:role/role-name"})
```

In these examples, *role\-name* is the role that you associated with your Amazon Redshift cluster, and *database\-name* and *table\-name* refer to an Amazon Redshift table in your Data Catalog\.

You can also specify a role when you use a dynamic frame and you use `copy_from_options`\. The syntax is similar, but you put the additional parameter in the *connection\-options* map:

```
connection_options = {  
    "url": "jdbc:redshift://host:port/redshift-database",
    "dbtable": "redshift-table",
    "user": "username",
    "password": "password",
    "redshiftTmpDir": args["TempDir"],
    "aws_iam_role": "arn:aws:iam::account-id:role/role-name"
}

df = glueContext.create_dynamic_frame_from_options("redshift", connection-options)
```

The options are similar when writing to Amazon Redshift:

```
connection_options = {
    "dbtable": "redshift-table",
    "database": "redshift-database",
    "aws_iam_role": "arn:aws:iam::account-id:role/role-name"
}

glueContext.write_dynamic_frame.from_jdbc_conf(
    frame = input-dynamic-frame, 
    catalog_connection = "connection-name", 
    connection_options = connection-options, 
    redshift_tmp_dir = args["TempDir"])
```