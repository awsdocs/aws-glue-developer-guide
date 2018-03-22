# Converting Semi\-Structured Schemas to Relational Schemas<a name="schema-relationalize"></a>

It's common to want to convert semi\-structured data into relational tables\. Conceptually, you are flattening a hierarchical schema to a relational schema\. AWS Glue can perform this conversion for you on\-the\-fly\.

Semi\-structured data typically contains mark\-up to identify entities within the data\. It can have nested data structures with no fixed schema\. For more information about semi\-structured data, see [Semi\-structured data](https://en.wikipedia.org/wiki/Semi-structured_data) in Wikipedia\. 

Relational data is represented by tables that consist of rows and columns\. Relationships between tables can be represented by a primary key \(PK\) to foreign key \(FK\) relationship\. For more information, see [Relational database](https://en.wikipedia.org/wiki/Relational_database) in Wikipedia\. 

AWS Glue uses crawlers to infer schemas for semi\-structured data\. It then transforms the data to a relational schema using an ETL \(extract, transform, and load\) job\. For example, you might want to parse JSON data from Amazon Simple Storage Service \(Amazon S3\) source files to Amazon Relational Database Service \(Amazon RDS\) tables\. Understanding how AWS Glue handles the differences between schemas can help you understand the transformation process\. 

This diagram shows how AWS Glue transforms a semi\-structured schema to a relational schema\.

![\[Flow showing conversion from semi-structured to relational schema.\]](http://docs.aws.amazon.com/glue/latest/dg/images/HowItWorks-schemaconversion.png)

The diagram illustrates the following:
+ Single value `A` converts directly to a relational column\.
+ The pair of values, `B1` and `B2`, convert to two relational columns\.
+ Structure `C`, with children `X` and `Y`, converts to two relational columns\.
+ Array `D[]` converts to a relational column with a foreign key \(FK\) that points to another relational table\. Along with a primary key \(PK\), the second relational table has columns that contain the offset and value of the items in the array\.