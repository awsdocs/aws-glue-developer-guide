# Populating the AWS Glue Data Catalog<a name="populate-data-catalog"></a>

The AWS Glue Data Catalog contains references to data that is used as sources and targets of your extract, transform, and load \(ETL\) jobs in AWS Glue\. To create your data warehouse, you must catalog this data\. The AWS Glue Data Catalog is an index to the location, schema, and runtime metrics of your data\. You use the information in the Data Catalog to create and monitor your ETL jobs\. Typically, you run a crawler to take inventory of the data in your data stores, but there are other ways to add metadata tables into your Data Catalog\.

You can add table definitions to the AWS Glue Data Catalog in the following ways:
+ Run a crawler that connects to one or more data stores, determines the data structures, and writes tables into the Data Catalog\. You can run your crawler on a schedule\. For more information, see [Cataloging Tables with a Crawler](add-crawler.md)\.
+ Use the AWS Glue console to create a table in the AWS Glue Data Catalog\. For more information, see [Working with Tables on the AWS Glue Console](console-tables.md)\.

  Use the `CreateTable` operation in the [AWS Glue API](aws-glue-api.md) to create a table in the AWS Glue Data Catalog\.

The following workflow diagram shows how AWS Glue crawlers interact with data stores and other elements to populate the Data Catalog\.

![\[Workflow showing how AWS Glue crawler populates the Data Catalog in 5 basic steps.\]](http://docs.aws.amazon.com/glue/latest/dg/images/PopulateCatalog-overview.png)

The following is the general workflow for how a crawler populates the AWS Glue Data Catalog:

1. A crawler runs any custom classifiers that you choose to infer the schema of your data\. You provide the code for custom classifiers, and they run in the order that you specify\.

   The first custom classifier to successfully recognize the structure of your data is used to create a schema\. Custom classifiers lower in the list are skipped\.

1. If no custom classifier matches your data's schema, built\-in classifiers try to recognize your data's schema\.

1. The crawler connects to the data store\. Some data stores require connection properties for crawler access\.

1. The inferred schema is created for your data\.

1. The crawler writes metadata to the Data Catalog\. A table definition contains metadata about the data in your data store\. The table is written to a database, which is a container of tables in the Data Catalog\. Attributes of a table include classification, which is a label created by the classifier that inferred the table schema\.

**Topics**
+ [Defining Tables in the AWS Glue Data Catalog](tables-described.md)
+ [Cataloging Tables with a Crawler](add-crawler.md)
+ [Defining a Database in Your Data Catalog](define-database.md)
+ [Adding Classifiers to a Crawler](add-classifier.md)
+ [Adding a Connection to Your Data Store](populate-add-connection.md)
+ [Populating the Data Catalog Using AWS CloudFormation Templates](populate-with-cloudformation-templates.md)