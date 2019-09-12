# AWS Glue Release Notes<a name="release-notes"></a>

The Glue version parameter is configured when adding or updating a job\. Glue version determines the versions of Apache Spark and Python that AWS Glue supports\. The Python version indicates the version supported for jobs of type Spark\. The following table lists the available Glue versions, the corresponding Spark and Python versions, and other changes in functionality\.

## AWS Glue Versions<a name="release-notes-versions"></a>


| Glue version | Supported Spark and Python versions | Changes in Functionality | 
| --- | --- | --- | 
| Glue 0\.9 |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/release-notes.html)  |  Jobs that were created without specifying a Glue version default to Glue 0\.9\.  | 
| Glue 1\.0 |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/release-notes.html)  |  You can maintain job bookmarks for Parquet and ORC formats in Glue ETL jobs \(using Glue Version 1\.0\)\. Previously, you were only able to bookmark common Amazon S3 source formats such as JSON, CSV, Apache Avro and XML in AWS Glue ETL jobs\.   | 