# What Is AWS Glue?<a name="what-is-glue"></a>

AWS Glue is a fully managed ETL \(extract, transform, and load\) service that makes it simple and cost\-effective to categorize your data, clean it, enrich it, and move it reliably between various data stores\. AWS Glue consists of a central metadata repository known as the AWS Glue Data Catalog, an ETL engine that automatically generates Python or Scala code, and a flexible scheduler that handles dependency resolution, job monitoring, and retries\. AWS Glue is serverless, so there’s no infrastructure to set up or manage\.

Use the AWS Glue console to discover data, transform it, and make it available for search and querying\. The console calls the underlying services to orchestrate the work required to transform your data\. You can also use the AWS Glue API operations to interface with AWS Glue services\. Edit, debug, and test your Python or Scala Apache Spark ETL code using a familiar development environment\.

For pricing information, see  [AWS Glue Pricing](https://aws.amazon.com/glue/pricing)\.

## When Should I Use AWS Glue?<a name="when-to-use-glue"></a>

**You can use AWS Glue to build a data warehouse to organize, cleanse, validate, and format data\.** You can transform and move AWS Cloud data  into your data store\. You can also load data from disparate sources into your data warehouse for regular reporting and analysis\. By storing it in a data warehouse, you integrate information from different parts of your business and provide a common source of data for decision making\. 

AWS Glue simplifies many tasks when you are building a data warehouse:
+ Discovers and catalogs metadata about your data stores into a central catalog\. You can process semi\-structured data, such as clickstream or process logs\.
+ Populates the AWS Glue Data Catalog with table definitions from scheduled crawler programs\. Crawlers call classifier logic to infer the schema, format, and data types of your data\. This metadata is stored as tables in the AWS Glue Data Catalog and used in the authoring process of your ETL jobs\.
+ Generates ETL scripts to transform, flatten, and enrich your data from source to target\.
+ Detects schema changes and adapts based on your preferences\.
+ Triggers your ETL jobs based on a schedule or event\. You can initiate jobs automatically to move your data into your data warehouse\. Triggers can be used to create a dependency flow between jobs\.
+ Gathers runtime metrics to monitor the activities of your data warehouse\.
+ Handles errors and retries automatically\.
+ Scales resources, as needed, to run your jobs\.

**You can use AWS Glue when you run serverless queries against your Amazon S3 data lake\.** AWS Glue can catalog your Amazon Simple Storage Service \(Amazon S3\) data, making it available for querying with Amazon Athena and Amazon Redshift Spectrum\. With crawlers, your metadata stays in sync with the underlying data\. Athena and Redshift Spectrum can directly query your Amazon S3 data lake using the AWS Glue Data Catalog\. With AWS Glue, you access and analyze data through one unified interface without loading it into multiple data silos\. 

**You can create event\-driven ETL pipelines with AWS Glue\.** You can run your ETL jobs as soon as new data becomes available in Amazon S3 by invoking your AWS Glue ETL jobs from an AWS Lambda function\. You can also register this new dataset in the AWS Glue Data Catalog as part of your ETL jobs\. 

**You can use AWS Glue to understand your data assets\.** You can store your data using various AWS services and still maintain a unified view of your data using the AWS Glue Data Catalog\. View the Data Catalog to quickly search and discover the datasets that you own, and maintain the relevant metadata in one central repository\. The Data Catalog also serves as a drop\-in replacement for your external Apache Hive Metastore\. 