# AWS Glue Data Catalog Support for Spark SQL Jobs<a name="aws-glue-programming-etl-glue-data-catalog-hive"></a>

The AWS Glue Data Catalog is an Apache Hive metastore\-compatible catalog\. You can configure your AWS Glue jobs and development endpoints to use the Data Catalog as an external Apache Hive metastore\. You can then directly run Apache Spark SQL queries against the tables stored in the Data Catalog\. AWS Glue dynamic frames integrate with the Data Catalog by default\. However, with this feature, Spark SQL jobs can start using the Data Catalog as an external Hive metastore\.

You can configure AWS Glue jobs and development endpoints by adding the `"--enable-glue-datacatalog": ""` argument to job arguments and development endpoint arguments respectively\. Passing this argument sets certain configurations in Spark that enable it to access the Data Catalog as an external Hive metastore\. It also [enables Hive support](https://spark.apache.org/docs/latest/api/java/org/apache/spark/sql/SparkSession.Builder.html#enableHiveSupport--) in the `SparkSession` object created in the AWS Glue job or development endpoint\. 

To enable the Data Catalog access, check the **Use Glue Data Catalog as the Hive metastore** check box in the **Catalog options** group on the **Add job** or **Add endpoint** page on the console\. Note that the IAM role used for the job or development endpoint should have `glue:CreateDatabase` permissions\. A database called "`default`" is created in the Data Catalog if it does not exist\. 

Lets look at an example of how you can use this feature in your Spark SQL jobs\. The following example assumes that you have crawled the US legislators dataset available at `s3://awsglue-datasets/examples/us-legislators`\.

To serialize/deserialize data from the tables defined in the Glue Data Catalog, Spark SQL needs the [Hive SerDe](https://cwiki.apache.org/confluence/display/Hive/SerDe) class for the format defined in the Glue Data Catalog in the classpath of the spark job\. 

SerDes for certain common formats are distributed by AWS Glue\. The following are the Amazon S3 links for these:
+ [JSON](https://s3-us-west-2.amazonaws.com/crawler-public/json/serde/json-serde.jar)
+ [XML](https://s3-us-west-2.amazonaws.com/crawler-public/xml/serde/hivexmlserde-1.0.5.3.jar)
+ [Grok](https://s3-us-west-2.amazonaws.com/crawler-public/grok/serde/AWSGlueHiveGrokSerDe-1.0-super.jar)

Add the JSON SerDe as an [extra JAR to the development endpoint](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-dev-endpoint.html#aws-glue-api-dev-endpoint-DevEndpointCustomLibraries)\. For jobs, you can add the SerDe using the `--extra-jars` argument in the arguments field\. For more information, see [Special Parameters Used by AWS Glue](aws-glue-programming-etl-glue-arguments.md)\. 

Here is an example input JSON to create a development endpoint with the Data Catalog enabled for Spark SQL\.

```
{
    "EndpointName": "Name",
    "RoleArn": "role_ARN",
    "PublicKey": "public_key_contents",
    "NumberOfNodes": 2,
    "Arguments": {
      "--enable-glue-datacatalog": ""
    },
    "ExtraJarsS3Path": "s3://crawler-public/json/serde/json-serde.jar"
}
```

Now query the tables created from the US legislators dataset using Spark SQL\.

```
>>> spark.sql("use legislators")
DataFrame[]
>>> spark.sql("show tables").show()
+-----------+------------------+-----------+
|   database|         tableName|isTemporary|
+-----------+------------------+-----------+
|legislators|        areas_json|      false|
|legislators|    countries_json|      false|
|legislators|       events_json|      false|
|legislators|  memberships_json|      false|
|legislators|organizations_json|      false|
|legislators|      persons_json|      false|
+-----------+------------------+-----------+
>>> spark.sql("describe memberships_json").show()
+--------------------+---------+-----------------+
|            col_name|data_type|          comment|
+--------------------+---------+-----------------+
|             area_id|   string|from deserializer|
|     on_behalf_of_id|   string|from deserializer|
|     organization_id|   string|from deserializer|
|                role|   string|from deserializer|
|           person_id|   string|from deserializer|
|legislative_perio...|   string|from deserializer|
|          start_date|   string|from deserializer|
|            end_date|   string|from deserializer|
+--------------------+---------+-----------------+
```

If the SerDe class for the format is not available in the job's classpath, you will see an error similar to the one shown below\.

```
>>> spark.sql("describe memberships_json").show()

Caused by: MetaException(message:java.lang.ClassNotFoundException Class org.openx.data.jsonserde.JsonSerDe not found)
    at org.apache.hadoop.hive.metastore.MetaStoreUtils.getDeserializer(MetaStoreUtils.java:399)
    at org.apache.hadoop.hive.ql.metadata.Table.getDeserializerFromMetaStore(Table.java:276)
    ... 64 more
```

To view only the distinct `organization_id`s from the `memberships` table, execute the following SQL query\.

```
>>> spark.sql("select distinct organization_id from memberships_json").show()
+--------------------+
|     organization_id|
+--------------------+
|d56acebe-8fdc-47b...|
|8fa6c3d2-71dc-478...|
+--------------------+
```

If you need to do the same with dynamic frames, execute the following\.

```
>>> memberships = glueContext.create_dynamic_frame.from_catalog(database="legislators", table_name="memberships_json")
>>> memberships.toDF().createOrReplaceTempView("memberships")
>>> spark.sql("select distinct organization_id from memberships").show()
+--------------------+
|     organization_id|
+--------------------+
|d56acebe-8fdc-47b...|
|8fa6c3d2-71dc-478...|
+--------------------+
```

While DynamicFrames are optimized for ETL operations, enabling Spark SQL to access the Data Catalog directly provides a concise way to execute complex SQL statements or port existing applications\.