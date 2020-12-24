# AWS Glue Release Notes<a name="release-notes"></a>

The AWS Glue version parameter is configured when adding or updating a job\. AWS Glue version determines the versions of Apache Spark and Python that AWS Glue supports\. The Python version indicates the version supported for jobs of type Spark\. The following table lists the available AWS Glue versions, the corresponding Spark and Python versions, and other changes in functionality\.

## AWS Glue Versions<a name="release-notes-versions"></a>


| AWS Glue version | Supported Spark and Python versions | Changes in Functionality | 
| --- | --- | --- | 
| AWS Glue 0\.9 |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/release-notes.html)  |  Jobs that were created without specifying a AWS Glue version default to AWS Glue 0\.9\.  | 
| AWS Glue 1\.0 |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/release-notes.html)  |  You can maintain job bookmarks for Parquet and ORC formats in AWS Glue ETL jobs \(using AWS Glue version 1\.0\)\. Previously, you were only able to bookmark common Amazon S3 source formats such as JSON, CSV, Apache Avro and XML in AWS Glue ETL jobs\.  When setting format options for ETL inputs and outputs, you can specify to use Apache Avro reader/writer format 1\.8 to support Avro logical type reading and writing \(using AWS Glue version 1\.0\)\. Previously, only the version 1\.7 Avro reader/writer format was supported\. The DynamoDB connection type supports a writer option \(using AWS Glue Version 1\.0\)\.  | 
| AWS Glue 2\.0 |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/release-notes.html)  |  In addition to the features provided in AWS Glue Version 1\.0, AWS Glue Version 2\.0 also provides: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/release-notes.html) For more information about AWS Glue 2\.0 features and limitations, see [Running Spark ETL Jobs with Reduced Startup Times](reduced-start-times-spark-etl-jobs.md)\.  | 