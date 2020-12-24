# Working with MongoDB Connections in ETL Jobs<a name="integrate-with-mongo-db"></a>

You can create a connection for MongoDB and then use that connection in your AWS Glue job\. The connection `url`, `username` and `password` are stored in the MongoDB connection\. Other options can be specified in your ETL job script using the `additionalOptions` parameter of `glueContext.getCatalogSource`\. The other options can include:
+ `database`: \(Required\) The MongoDB database to read from\.
+ `collection`: \(Required\) The MongoDB collection to read from\.

By placing the `database` and `collection` information inside the ETL job script, you can use the same connection for in multiple jobs\.

1. Create an AWS Glue Data Catalog connection for the MongoDB data source\. See ["connectionType": "mongodb"](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-connect.html#aws-glue-programming-etl-connect-mongodb) for a description of the connection parameters\. You can create the connection using the console, APIs or CLI\.

1. Create a database in the AWS Glue Data Catalog to store the table definitions for your MongoDB data\. See [Defining a Database in Your Data Catalog](define-database.md) for more information\.

1. Create a crawler that crawls the data in the MongoDB using the information in the connection to connect to the MongoDB\. The crawler creates the tables in the AWS Glue Data Catalog that describe the tables in the MongoDB database that you use in your job\. See [Defining Crawlers](add-crawler.md) for more information\.

1. Create a job with a custom script\. You can create the job using the console, APIs or CLI\. For more information, see [Adding Jobs in AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/add-job.html)\.

1. Choose the data targets for your job\. The tables that represent the data target can be defined in your Data Catalog, or your job can create the target tables when it runs\. You choose a target location when you author the job\. If the target requires a connection, the connection is also referenced in your job\. If your job requires multiple data targets, you can add them later by editing the script\.

1. Customize the job\-processing environment by providing arguments for your job and generated script\. 

   Here is an example of creating a `DynamicFrame` from the MongoDB database based on the table structure defined in the Data Catalog\. The code uses `additionalOptions` to provide the additional data source information:

------
#### [  Scala  ]

   ```
   val resultFrame: DynamicFrame = glueContext.getCatalogSource(
           database = catalogDB, 
           tableName = catalogTable, 
           additionalOptions = JsonOptions(Map("database" -> DATABASE_NAME, 
                   "collection" -> COLLECTION_NAME))
         ).getDynamicFrame()
   ```

------
#### [  Python  ]

   ```
   glue_context.create_dynamic_frame_from_catalog(
           database = catalogDB,
           table_name = catalogTable,
           additional_options = {"database":"database_name", 
               "collection":"collection_name"})
   ```

------

1. Run the job, either on\-demand or through a trigger\.