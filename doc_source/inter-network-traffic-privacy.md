# Internetwork Traffic Privacy<a name="inter-network-traffic-privacy"></a>

AWS Glue uses other AWS services to orchestrate your extract, transform, and load \(ETL\) jobs to build a data warehouse\. AWS Glue calls API operations to transform your data, create runtime logs, store your job logic, and create notifications to help you monitor your job runs\. For more information, see [AWS Glue Dependency on Other AWS Services](dependency-on-other-services.md)\.

The AWS Glue console connects these services into a managed application\. The console performs administrative and job development operations on your behalf\. You supply credentials and other properties to AWS Glue to access your data sources and write to your data warehouse\. AWS Glue takes care of provisioning and managing resources that are required to run your workload\. When resources are required, to reduce startup time, AWS Glue uses an instance from its warm pool of instances to run your workload\.

To start using AWS Glue, sign into the AWS Management Console\. Under the **Analytics** category, choose **Glue**\. AWS Glue automatically discovers and profiles your data via the AWS Glue Data Catalog\. It recommends and generates ETL code to transform your source data into target schemas\. It then runs the ETL jobs on a fully managed, scale\-out Apache Spark environment to load your data into its destination\. It also enables you to set up, orchestrate, and monitor complex data flows\.

You can create jobs using table definitions in your Data Catalog\. Jobs consist of scripts that contain programming logic that performs the transformation\. You use triggers to initiate jobs either on a schedule or as a result of a specified event\. You determine where your target data resides and which source data populates your target\. With your input, AWS Glue generates the code that's required to transform your data from source to target\. You can also provide scripts in the AWS Glue console or API to process your data\.

**Topics**
+ [AWS Glue Console](#inter-network-traffic-privacy.console)
+ [AWS Glue Data Catalog](#inter-network-traffic-privacy.data-catalog)
+ [AWS Glue Crawlers and Classifiers](#inter-network-traffic-privacy.crawlers-classifiers)
+ [AWS Glue ETL Operations](#inter-network-traffic-privacy.etl)
+ [AWS Glue Jobs System](#inter-network-traffic-privacy.jobs)

## AWS Glue Console<a name="inter-network-traffic-privacy.console"></a>

Use the AWS Glue console to define and orchestrate your ETL workflow\. The console calls several API operations in the AWS Glue Data Catalog and AWS Glue jobs system to perform the following tasks:
+ Define AWS Glue objects such as jobs, tables, crawlers, and connections
+ Schedule when crawlers run
+ Define events or schedules for job triggers
+ Search and filter lists of AWS Glue objects
+ Edit transformation scripts AWS Glue Data Catalog

## AWS Glue Data Catalog<a name="inter-network-traffic-privacy.data-catalog"></a>

The AWS Glue Data Catalog is your persistent metadata store\. It is a managed service that lets you store, annotate, and share metadata in the AWS Cloud in the same way you would in an Apache Hive metastore\.

Each AWS account has one AWS Glue Data Catalog\. It provides a uniform repository where disparate systems can store and find metadata to keep track of data in data silos, and use that metadata to query and transform the data\.

AWS Identity and Access Management \(IAM\) policies control access to data sources managed by the AWS Glue Data Catalog\. These policies allow different groups in your enterprise to safely publish data to the wider organization while protecting sensitive information\. IAM policies let you clearly and consistently define which users have access to which data, regardless of its location\. 

The Data Catalog also provides comprehensive audit and governance capabilities, with schema change tracking, lineage of data, and data access controls\. You can audit changes to data schemas and track the movement of data across systems\. This helps ensure that data is not inappropriately modified or inadvertently shared\.

For information about how to use the AWS Glue Data Catalog, see [Populating the AWS Glue Data Catalog](populate-data-catalog.md)\. For information about how to program using the Data Catalog API, see [Catalog API](aws-glue-api-catalog.md)\.

## AWS Glue Crawlers and Classifiers<a name="inter-network-traffic-privacy.crawlers-classifiers"></a>

AWS Glue sets up crawlers that scan data in all kinds of repositories\. They classify the data, extract schema information from it, and store the metadata automatically in the AWS Glue Data Catalog\. From there, it can be used to guide ETL operations\.

For information about how to set up crawlers and classifiers, see [Defining Crawlers](add-crawler.md)\. For information about how to program crawlers and classifiers using the AWS Glue API, see [Crawlers and Classifiers API](aws-glue-api-crawler.md)\.

## AWS Glue ETL Operations<a name="inter-network-traffic-privacy.etl"></a>

Using metadata in the Data Catalog, AWS Glue generates Scala or PySpark \(the Python API for Apache Spark\) scripts with AWS Glue extensions\. You can use and modify these scripts to perform various ETL operations\. You can extract, clean, and transform raw data, and then store the result in a different repository, where it can be queried and analyzed\. Such a script might convert a CSV file into a relational form and save it in Amazon Redshift\.

For more information about how to use AWS Glue ETL capabilities, see [Programming ETL Scripts](aws-glue-programming.md)\.

## AWS Glue Jobs System<a name="inter-network-traffic-privacy.jobs"></a>

The AWS Glue jobs system provides managed infrastructure to orchestrate your ETL workflow\. You create jobs in AWS Glue that automate scripts used to extract, transform, and transfer data to different locations\. Jobs can be scheduled and chained, or they can be triggered by events such as the arrival of new data\.

For more information about using the AWS Glue jobs system, see [Running and Monitoring AWS Glue](monitor-glue.md)\. For information about programming using the AWS Glue jobs system API, see [Jobs API](aws-glue-api-jobs.md)\.