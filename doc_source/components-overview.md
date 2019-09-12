# AWS Glue Components<a name="components-overview"></a>

AWS Glue provides a console and API operations to set up and manage your extract, transform, and load \(ETL\) workload\. You can use API operations through several language\-specific SDKs and the AWS Command Line Interface \(AWS CLI\)\. For information about using the AWS CLI, see [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)\.

AWS Glue uses the AWS Glue Data Catalog to store metadata about data sources, transforms, and targets\. The Data Catalog is a drop\-in replacement for the Apache Hive Metastore\. The AWS Glue Jobs system provides a managed infrastructure for defining, scheduling, and running ETL operations on your data\. For more information about the AWS Glue API, see [AWS Glue API](aws-glue-api.md)\.

## AWS Glue Console<a name="console-intro"></a>

You use the AWS Glue console to define and orchestrate your ETL workflow\. The console calls several API operations in the AWS Glue Data Catalog and AWS Glue Jobs system to perform the following tasks:
+ Define AWS Glue objects such as jobs, tables, crawlers, and connections\.
+ Schedule when crawlers run\.
+ Define events or schedules for job triggers\.
+ Search and filter lists of AWS Glue objects\.
+ Edit transformation scripts\.

## AWS Glue Data Catalog<a name="data-catalog-intro"></a>

The AWS Glue Data Catalog is your persistent metadata store\. It is a managed service that lets you store, annotate, and share metadata in the AWS Cloud in the same way you would in an Apache Hive metastore\.

Each AWS account has one AWS Glue Data Catalog per AWS region\. It provides a uniform repository where disparate systems can store and find metadata to keep track of data in data silos, and use that metadata to query and transform the data\.

You can use AWS Identity and Access Management \(IAM\) policies to control access to the data sources managed by the AWS Glue Data Catalog\. These policies allow different groups in your enterprise to safely publish data to the wider organization while protecting sensitive information\. IAM policies let you clearly and consistently define which users have access to which data, regardless of its location\.

For information about how to use the AWS Glue Data Catalog, see [Populating the AWS Glue Data Catalog](populate-data-catalog.md)\. For information about how to program using the Data Catalog API, see [Catalog API](aws-glue-api-catalog.md)\.

Other AWS services and open source projects can use the AWS Glue Data Catalog:
+ Amazon Athena – for more information, see [Understanding Tables, Databases, and the Data Catalog](https://docs.aws.amazon.com/athena/latest/ug/understanding-tables-databases-and-the-data-catalog.html) in the Amazon Athena User Guide\.
+ Amazon Redshift Spectrum – for more information, see [Using Amazon Redshift Spectrum to Query External Data](https://docs.aws.amazon.com/redshift/latest/dg/c-using-spectrum.html) in the Amazon Redshift Database Developer Guide\.
+ Amazon EMR – for more information, see [Use Resource\-Based Policies for Amazon EMR Access to AWS Glue Data Catalog](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-iam-roles-glue.html) in the Amazon EMR Management Guide\.
+ AWS Glue Data Catalog Client for Apache Hive Metastore – for more information about this GitHub project, see [AWS Glue Data Catalog Client for Apache Hive Metastore](https://github.com/awslabs/aws-glue-data-catalog-client-for-apache-hive-metastore)\.

## AWS Glue Crawlers and Classifiers<a name="crawling-intro"></a>

AWS Glue also lets you set up crawlers that can scan data in all kinds of repositories, classify it, extract schema information from it, and store the metadata automatically in the AWS Glue Data Catalog\. From there it can be used to guide ETL operations\.

For information about how to set up crawlers and classifiers, see [Defining Crawlers](add-crawler.md)\. For information about how to program crawlers and classifiers using the AWS Glue API, see [Crawlers and Classifiers API](aws-glue-api-crawler.md)\.

## AWS Glue ETL Operations<a name="etl-script-intro"></a>

Using the metadata in the Data Catalog, AWS Glue can autogenerate Scala or PySpark \(the Python API for Apache Spark\) scripts with AWS Glue extensions that you can use and modify to perform various ETL operations\. For example, you can extract, clean, and transform raw data, and then store the result in a different repository, where it can be queried and analyzed\. Such a script might convert a CSV file into a relational form and save it in Amazon Redshift\.

For more information about how to use AWS Glue ETL capabilities, see [Programming ETL Scripts](aws-glue-programming.md)\.

## The AWS Glue Jobs System<a name="job-orchestration-intro"></a>

The AWS Glue Jobs system provides managed infrastructure to orchestrate your ETL workflow\. You can create jobs in AWS Glue that automate the scripts you use to extract, transform, and transfer data to different locations\. Jobs can be scheduled and chained, or they can be triggered by events such as the arrival of new data\.

For more information about using the AWS Glue Jobs system, see [Running and Monitoring AWS Glue](monitor-glue.md)\. For information about programming using the AWS Glue Jobs system API, see [Jobs API](aws-glue-api-jobs.md)\.