# AWS Glue Concepts<a name="components-key-concepts"></a>

The following diagram shows the architecture of an AWS Glue environment\.

![\[The basic concepts populating your Data Catalog and processing ETL dataflow in AWS Glue.\]](http://docs.aws.amazon.com/glue/latest/dg/images/HowItWorks-overview.png)

You define *jobs* in AWS Glue to accomplish the work that's required to extract, transform, and load \(ETL\) data from a data source to a data target\. You typically perform the following actions:
+ You define a *crawler* to populate your AWS Glue Data Catalog with metadata table definitions\. You point your crawler at a data store, and the crawler creates table definitions in the Data Catalog\.

  In addition to table definitions, the AWS Glue Data Catalog contains other metadata that is required to define ETL jobs\. You use this metadata when you define a job to transform your data\.
+ AWS Glue can generate a script to transform your data\. Or, you can provide the script in the AWS Glue console or API\.
+ You can run your job on demand, or you can set it up to start when a specified *trigger* occurs\. The trigger can be a time\-based schedule or an event\.

  When your job runs, a script extracts data from your data source, transforms the data, and loads it to your data target\. The script runs in an Apache Spark environment in AWS Glue\.

**Important**  
Tables and databases in AWS Glue are objects in the AWS Glue Data Catalog\. They contain metadata; they don't contain data from a data store\.


|  | 
| --- |
|  **Text\-based data, such as CSVs, must be encoded in `UTF-8` for AWS Glue to process it successfully\. For more information, see [UTF\-8](https://en.wikipedia.org/wiki/UTF-8) in Wikipedia\.**   | 

## AWS Glue Terminology<a name="components-major"></a>

AWS Glue relies on the interaction of several components to create and manage your data warehouse workflow\.

### AWS Glue Data Catalog<a name="components-data-catalog"></a>

The persistent metadata store in AWS Glue\. It contains table definitions, job definitions, and other control information to manage your AWS Glue environment\. Each AWS account has one AWS Glue Data Catalog per region\. 

### Classifier<a name="components-classifier"></a>

Determines the schema of your data\. AWS Glue provides classifiers for common file types, such as CSV, JSON, AVRO, XML, and others\. It also provides classifiers for common relational database management systems using a JDBC connection\. You can write your own classifier by using  a grok pattern or by specifying a row tag in an XML document\.

### Connection<a name="components-connection"></a>

Contains the properties that are required to connect to your data store\.

### Crawler<a name="components-crawler"></a>

A program that connects to a data store \(source or target\), progresses through a prioritized list of classifiers to determine the schema for your data, and then creates metadata tables in the AWS Glue Data Catalog\.

### Database<a name="components-database"></a>

A set of associated Data Catalog table definitions organized into a logical group in AWS Glue\.

### Data store, data source, data target<a name="components-data-store"></a>

A *data store* is a repository for persistently storing your data\. Examples include Amazon S3 buckets and relational databases\. A *data source* is a data store that is used as input to a process or transform\. A *data target* is a data store that a process or transform writes to\.

### Development endpoint<a name="components-development-endpoint"></a>

An environment that you can use to develop and test your AWS Glue ETL scripts\.

### Job<a name="components-job"></a>

The business logic that is required to perform ETL work\. It is composed of a transformation script, data sources, and data targets\. Job runs are initiated by triggers that can be scheduled or triggered by events\.

### Notebook server<a name="components-notebook-server"></a>

A web\-based environment that you can use to run your PySpark statements\. PySpark is a Python dialect for ETL programming\. For more information, see [Apache Zeppelin](http://zeppelin.apache.org/)\. You can set up a notebook server on a development endpoint to run PySpark statements with AWS Glue extensions\. 

### Script<a name="components-script"></a>

Code that extracts data from sources, transforms it, and loads it into targets\. AWS Glue generates PySpark or Scala scripts\.

### Table<a name="components-table"></a>

The metadata definition that represents your data\. Whether your data is in an Amazon Simple Storage Service \(Amazon S3\) file, an Amazon Relational Database Service \(Amazon RDS\) table, or another set of data, a table defines the schema of your data\. A table in the AWS Glue Data Catalog consists of the names of columns, data type definitions, partition information, and other metadata about a base dataset\. The schema of your data is represented in your AWS Glue table definition\. The actual data remains in its original data store, whether it be in a file or a relational database table\. AWS Glue catalogs your files and relational database tables in the AWS Glue Data Catalog\. They are used as sources and targets when you create an ETL job\.

### Transform<a name="components-transform"></a>

The code logic that is used to manipulate your data into a different format\.

### Trigger<a name="components-trigger"></a>

Initiates an ETL job\. Triggers can be defined based on a scheduled time or an event\.