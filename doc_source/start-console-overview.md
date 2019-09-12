# AWS Glue Console Workflow Overview<a name="start-console-overview"></a>

With AWS Glue, you store metadata in the AWS Glue Data Catalog\. You use this metadata to orchestrate ETL jobs that transform data sources and load your data warehouse\. The following steps describe the general workflow and some of the choices that you make when working with AWS Glue\.

1. Populate the AWS Glue Data Catalog with table definitions\.

   In the console, you can add a crawler to populate the AWS Glue Data Catalog\. You can start the **Add crawler** wizard from the list of tables or the list of crawlers\. You choose one or more data stores for your crawler to access\. You can also create a schedule to determine the frequency of running your crawler\.

   Optionally, you can provide a custom classifier that infers the schema of your data\. You can create custom classifiers using  a grok pattern\. However, AWS Glue provides built\-in classifiers that are automatically used by crawlers if a custom classifier does not recognize your data\. When you define a crawler, you don't have to select a classifier\. For more information about classifiers in AWS Glue, see [Adding Classifiers to a Crawler](add-classifier.md)\. 

   Crawling some types of data stores requires a connection that provides authentication and location information\. If needed, you can create a connection that provides this required information in the AWS Glue console\.

   The crawler reads your data store and creates data definitions and named tables in the AWS Glue Data Catalog\. These tables are organized into a database of your choosing\. You can also populate the Data Catalog with manually created tables\. With this method, you provide the schema and other metadata to create table definitions in the Data Catalog\. Because this method can be a bit tedious and error prone, it's often better to have a crawler create the table definitions\.

   For more information about populating the AWS Glue Data Catalog with table definitions, see [Defining Tables in the AWS Glue Data Catalog](tables-described.md)\.

1. Define a job that describes the transformation of data from source to target\.

   Generally, to create a job, you have to make the following choices:
   + Pick a table from the AWS Glue Data Catalog to be the source of the job\. Your job uses this table definition to access your data store and interpret the format of your data\.
   + Pick a table or location from the AWS Glue Data Catalog to be the target of the job\. Your job uses this information to access your data store\.
   + Tell AWS Glue to generate a PySpark script to transform your source to target\. AWS Glue generates the code to call built\-in transforms to convert data from its source schema to target schema format\. These transforms perform operations such as copy data, rename columns, and filter data to transform data as necessary\. You can modify this script in the AWS Glue console\.

   For more information about defining jobs in AWS Glue, see [Authoring Jobs in AWS Glue](author-job.md)\.

1. Run your job to transform your data\.

   You can run your job on demand, or start it based on a one of these trigger types:
   + A trigger that is based on a cron schedule\.
   + A trigger that is event\-based; for example, the successful completion of another job can start an AWS Glue job\.
   + A trigger that starts a job on demand\.

   For more information about triggers in AWS Glue, see [Triggering Jobs in AWS Glue](trigger-job.md)\.

1. Monitor your scheduled crawlers and triggered jobs\.

   Use the AWS Glue console to view the following:
   + Job run details and errors\.
   + Crawler run details and errors\.
   + Any notifications about AWS Glue activities

   For more information about monitoring your crawlers and jobs in AWS Glue, see [Running and Monitoring AWS Glue](monitor-glue.md)\.