# Using a MongoDB Connection<a name="connection-mongodb"></a>

After creating a connection for MongoDB, you can use this connection in your ETL job\. You create a table in the AWS Glue Data Catalog and specify the MongoDB connection for the `connection` attribute of the table\. The connection `url`, `username` and `password` are stored in the MongoDB connection\. Other options can be specified in the job script as additional options\. The other options can include: 
+ `"database"`: \(Required\) The MongoDB database to read from\.
+ `"collection"`: \(Required\) The MongoDB collection to read from\.
+ `"ssl"`: \(Optional\) If `true`, then AWS Glue initiates an SSL connection\. The default value is `false`\.
+ `"ssl.domain_match"`: \(Optional\) If `true` and `ssl` is `true`, then AWS Glue performs a domain match check\. The default value is `true`\.
+ `"batchSize"`: \(Optional\): The number of documents to return per batch, used within the cursor of internal batches\.
+ `"partitioner"`: \(Optional\): The class name of the partitioner for reading input data from MongoDB\. The connector provides the following partitioners:
  + `MongoDefaultPartitioner` \(default\) 
  + `MongoSamplePartitioner` \(Requires MongoDB 3\.2 or later\)
  + `MongoShardedPartitioner`
  + `MongoSplitVectorPartitioner`
  + `MongoPaginateByCountPartitioner` 
  + `MongoPaginateBySizePartitioner`
+ `"partitionerOptions"`: \(Optional\): Options for the designated partitioner\. The following options are supported for each partitioner:
  + `MongoSamplePartitioner`–`partitionKey`, `partitionSizeMB`, and `samplesPerPartition`
  + `MongoShardedPartitioner`–`shardkey`
  + `MongoSplitVectorPartitioner`–`partitionKey` and `partitionSizeMB`
  + `MongoPaginateByCountPartitioner`–`partitionKey` and `numberOfPartitions`
  + `MongoPaginateBySizePartitioner`–`partitionKey` and `partitionSizeMB`

For more information about these options, see [https://docs.mongodb.com/spark-connector/master/configuration/#partitioner-conf](https://docs.mongodb.com/spark-connector/master/configuration/#partitioner-conf)\.

The following examples demonstrate how to get a `DynamicFrame` from the catalog source\.

------
#### [ Python ]

```
glue_context.create_dynamic_frame_from_catalog(
    database = nameSpace,
    table_name = tableName,
    additional_options = {"database":"database_name", 
            "collection":"collection_name"})
```

------
#### [ Scala ]

```
 val resultFrame: DynamicFrame = glueContext.getCatalogSource(
         database = nameSpace,
         tableName = tableName,
         additionalOptions = JsonOptions(Map("database" -> DATABASE_NAME, 
            "collection" -> COLLECTION_NAME))
       ).getDynamicFrame()
```

------