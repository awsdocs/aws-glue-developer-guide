# Connection Types and Options for ETL in AWS Glue<a name="aws-glue-programming-etl-connect"></a>

Various AWS Glue PySpark and Scala methods and transforms specify connection parameters using a `connectionType` parameter and a `connectionOptions` parameter\.

The `connectionType` parameter can take the following values, and the associated "connectionOptions" parameter values for each type are documented below:

In general, these are for ETL input and do not apply to ETL sinks\.
+ ["connectionType": "s3"](#aws-glue-programming-etl-connect-s3): Designates a connection to Amazon Simple Storage Service \(Amazon S3\)\.
+ ["connectionType": "parquet"](#aws-glue-programming-etl-connect-parquet): Designates a connection to files stored in Amazon S3 in the [Apache Parquet](https://parquet.apache.org/documentation/latest/) file format\.
+ ["connectionType": "orc"](#aws-glue-programming-etl-connect-orc): Designates a connection to files stored in Amazon S3 in the [Apache Hive Optimized Row Columnar \(ORC\)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC) file format\.
+ ["connectionType": "mysql"](#aws-glue-programming-etl-connect-jdbc): Designates a connection to a [MySQL](https://www.mysql.com/) database \(see [JDBC connectionType values](#aws-glue-programming-etl-connect-jdbc)\)\.
+ ["connectionType": "redshift"](#aws-glue-programming-etl-connect-jdbc): Designates a connection to an [Amazon Redshift](https://aws.amazon.com/documentation/redshift/?id=docs_gateway) database \(see [JDBC connectionType values](#aws-glue-programming-etl-connect-jdbc)\)\.
+ ["connectionType": "oracle"](#aws-glue-programming-etl-connect-jdbc): Designates a connection to an Oracle database \(see [JDBC connectionType values](#aws-glue-programming-etl-connect-jdbc)\)\.
+ ["connectionType": "sqlserver"](#aws-glue-programming-etl-connect-jdbc): Designates a connection to a Microsoft SQL Server database \(see [JDBC connectionType values](#aws-glue-programming-etl-connect-jdbc)\)\.
+ ["connectionType": "postgresql"](#aws-glue-programming-etl-connect-jdbc): Designates a connection to a [PostgreSQL](https://www.postgresql.org/) database \(see [JDBC connectionType values](#aws-glue-programming-etl-connect-jdbc)\)\.
+ ["connectionType": "dynamodb"](#aws-glue-programming-etl-connect-dynamodb): Designates a connection to Amazon DynamoDB; \(DynamoDB\)\.

## "connectionType": "s3"<a name="aws-glue-programming-etl-connect-s3"></a>

Designates a connection to Amazon Simple Storage Service \(Amazon S3\)\.

Use the following `connectionOptions` with `"connectionType": "s3"`:
+ `"paths"`: \(Required\) A list of the Amazon S3 paths from which to read\.
+ `"exclusions"`: \(Optional\) A string containing a JSON list of Unix\-style glob patterns to exclude\. for example "\[\\"\*\*\.pdf\\"\]" would exclude all pdf files\. More information about the glob syntax supported by AWS Glue can be found at [Using Include and Exclude Patterns](https://docs.aws.amazon.com/glue/latest/dg/add-crawler.html#crawler-data-stores-exclude)\.
+ `"compressionType"`: or "`compression`": \(Optional\) Specifies how the data is compressed\. Use `"compressionType"` for Amazon S3 sources and `"compression"` for Amazon S3 targets\. This is generally not necessary if the data has a standard file extension\. Possible values are `"gzip"` and `"bzip"`\)\.
+ `"groupFiles"`: \(Optional\) Grouping files is enabled by default when the input contains more than 50,000 files\. To enable grouping with fewer than 50,000 files, set this parameter to `"inPartition"`\. To disable grouping when there are more than 50,000 files, set this parameter to `"none"`\.
+ `"groupSize"`: \(Optional\) The target group size in bytes\. The default is computed based on the input data size and the size of your cluster\. When there are fewer than 50,000 input files, `"groupFiles"` must be set to `"inPartition"` for this to take effect\.
+ `"recurse"`: \(Optional\) If set to true, recursively reads files in all subdirectories under the specified paths\.
+ `"maxBand"`: \(Optional, Advanced\) This option controls the duration in seconds after which s3 listing is likely to be consistent\. Files with modification timestamps falling within the last `maxBand` seconds are tracked specially when using JobBookmarks to account for S3 eventual consistency\. Most users do not need to set this option\. The default is 900 seconds\.
+ `"maxFilesInBand"`: \(Optional, Advanced\) This option specifies the maximum number of files to save from the last `maxBand` seconds\. If this number is exceeded, extra files are skipped and only processed in the next job run\. Most users do not need to set this option\.

## "connectionType": "parquet"<a name="aws-glue-programming-etl-connect-parquet"></a>

Designates a connection to files stored in Amazon Simple Storage Service \(Amazon S3\) in the [Apache Parquet](https://parquet.apache.org/documentation/latest/) file format\.

Use the following `connectionOptions` with `"connectionType": "parquet"`:
+ `paths`: \(Required\) A list of the Amazon S3 paths from which to read\.
+ *\(Other option name/value pairs\)*: Any additional options, including formatting options, are passed directly to the SparkSQL DataSource\. For more information, see [Redshift data source for Spark](https://github.com/databricks/spark-redshift)\.

## "connectionType": "orc"<a name="aws-glue-programming-etl-connect-orc"></a>

Designates a connection to files stored in Amazon S3 in the [Apache Hive Optimized Row Columnar \(ORC\)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC) file format\.

Use the following `connectionOptions` with `"connectionType": "orc"`:
+ `paths`: \(Required\) A list of the Amazon S3 paths from which to read\.
+ *\(Other option name/value pairs\)*: Any additional options, including formatting options, are passed directly to the SparkSQL DataSource\. For more information, see [Redshift data source for Spark](https://github.com/databricks/spark-redshift)\.

## JDBC connectionType values<a name="aws-glue-programming-etl-connect-jdbc"></a>

These include the following:
+ `"connectionType": "mysql"`: Designates a connection to a [MySQL](https://www.mysql.com/) database\.
+ `"connectionType": "redshift"`: Designates a connection to an [Amazon Redshift](https://aws.amazon.com/documentation/redshift/?id=docs_gateway) database\.
+ `"connectionType": "oracle"`: Designates a connection to an Oracle database\.
+ `"connectionType": "sqlserver"`: Designates a connection to a Microsoft SQL Server database\.
+ `"connectionType": "postgresql"`: Designates a connection to a [PostgreSQL](https://www.postgresql.org/) database\.

Use these `connectionOptions` with JDBC connections:
+ `"url"`: \(Required\) The JDBC URL for the database\.
+ `"dbtable"`: The database table to read from\. For JDBC data stores that support schemas within a database, specify `schema.table-name`\. If a schema is not provided, then the default "public" schema is used\.
+ `"redshiftTmpDir"`: \(Required for Amazon Redshift, optional for other JDBC types\) The Amazon S3 path where temporary data can be staged when copying out of the database\.
+ `"user"`: \(Required\) The username to use when connecting\.
+ `"password"`: \(Required\) The password to use when connecting\.

All other option name/value pairs that are included in `connectionOptions` for a JDBC connection, including formatting options, are passed directly to the underlying SparkSQL DataSource\. For more information, see [Redshift data source for Spark](https://github.com/databricks/spark-redshift)\.

## "connectionType": "dynamodb"<a name="aws-glue-programming-etl-connect-dynamodb"></a>

Designates a connection to Amazon DynamoDB \(DynamoDB\)\.

Use the following `connectionOptions` with `"connectionType": "dynamodb"`:
+ `"dynamodb.input.tableName"`: \(Required\) The DynamoDB table from which to read\.
+ `"dynamodb.throughput.read.percent"`: \(Optional\) The percentage of read capacity units \(RCU\) to use\. The default is set to "0\.5"\. Acceptable values are from "0\.1" to "1\.5", inclusive\.
+ `"dynamodb.splits"`: \(Optional\) This parameter defines how many splits we partition this DynamoDB table into while reading\. The default is set to "1"\. Acceptable values are from "1" to "1,000,000", inclusive\.