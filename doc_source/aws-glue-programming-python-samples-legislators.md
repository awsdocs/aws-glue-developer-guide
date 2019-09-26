# Code Example: Joining and Relationalizing Data<a name="aws-glue-programming-python-samples-legislators"></a>

This example uses a dataset that was downloaded from [http://everypolitician\.org/](http://everypolitician.org/) to the `sample-dataset` bucket in Amazon Simple Storage Service \(Amazon S3\): `s3://awsglue-datasets/examples/us-legislators/all`\. The dataset contains data in JSON format about United States legislators and the seats that they have held in the US House of Representatives and Senate, and has been modified slightly and made available in a public Amazon S3 bucket for purposes of this tutorial\.

You can find the source code for this example in the `join_and_relationalize.py` file in the [AWS Glue samples repository](https://github.com/awslabs/aws-glue-samples) on the GitHub website\.

Using this data, this tutorial shows you how to do the following:
+ Use an AWS Glue crawler to classify objects that are stored in a public Amazon S3 bucket and save their schemas into the AWS Glue Data Catalog\.
+ Examine the table metadata and schemas that result from the crawl\.
+ Write a Python extract, transfer, and load \(ETL\) script that uses the metadata in the Data Catalog to do the following:
  + Join the data in the different source files together into a single data table \(that is, denormalize the data\)\.
  + Filter the joined table into separate tables by type of legislator\.
  + Write out the resulting data to separate Apache Parquet files for later analysis\.

The easiest way to debug Python or PySpark scripts is to create a development endpoint and run your code there\. We recommend that you start by setting up a development endpoint to work in\. For more information, see [Viewing Development Endpoint Properties](console-development-endpoint.md)\.

## Step 1: Crawl the Data in the Amazon S3 Bucket<a name="aws-glue-programming-python-samples-legislators-crawling"></a>

1. Sign in to the AWS Management Console, and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. Following the steps in [Working with Crawlers on the AWS Glue Console](console-crawlers.md), create a new crawler that can crawl the `s3://awsglue-datasets/examples/us-legislators/all` dataset into a database named `legislators` in the AWS Glue Data Catalog\. The example data is already in this public Amazon S3 bucket\.

1. Run the new crawler, and then check the `legislators` database\. 

   The crawler creates the following metadata tables:
   + `persons_json`
   + `memberships_json`
   + `organizations_json`
   + `events_json`
   + `areas_json`
   + `countries_r_json`

   This is a semi\-normalized collection of tables containing legislators and their histories\.

## Step 2: Add Boilerplate Script to the Development Endpoint Notebook<a name="aws-glue-programming-python-samples-legislators-boilerplate"></a>

Paste the following boilerplate script into the development endpoint notebook to import the AWS Glue libraries that you need, and set up a single `GlueContext`:

```
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job

glueContext = GlueContext(SparkContext.getOrCreate())
```

## Step 3: Examine the Schemas in the Data Catalog<a name="aws-glue-programming-python-samples-legislators-schemas"></a>

Next, you can easily examine the schemas that the crawler recorded in the AWS Glue Data Catalog\. For example, to see the schema of the `persons_json` table, add the following in your notebook:

```
persons = glueContext.create_dynamic_frame.from_catalog(
             database="legislators",
             table_name="persons_json")
print "Count: ", persons.count()
persons.printSchema()
```

Here's the output from the print calls:

```
Count:  1961
root
|-- family_name: string
|-- name: string
|-- links: array
|    |-- element: struct
|    |    |-- note: string
|    |    |-- url: string
|-- gender: string
|-- image: string
|-- identifiers: array
|    |-- element: struct
|    |    |-- scheme: string
|    |    |-- identifier: string
|-- other_names: array
|    |-- element: struct
|    |    |-- note: string
|    |    |-- name: string
|    |    |-- lang: string
|-- sort_name: string
|-- images: array
|    |-- element: struct
|    |    |-- url: string
|-- given_name: string
|-- birth_date: string
|-- id: string
|-- contact_details: array
|    |-- element: struct
|    |    |-- type: string
|    |    |-- value: string
|-- death_date: string
```

Each person in the table is a member of some US congressional body\.

To view the schema of the `memberships_json` table, type the following:

```
memberships = glueContext.create_dynamic_frame.from_catalog(
                 database="legislators",
                 table_name="memberships_json")
print "Count: ", memberships.count()
memberships.printSchema()
```

The output is as follows:

```
Count:  10439
root
|-- area_id: string
|-- on_behalf_of_id: string
|-- organization_id: string
|-- role: string
|-- person_id: string
|-- legislative_period_id: string
|-- start_date: string
|-- end_date: string
```

The `organizations` are parties and the two chambers of Congress, the Senate and House of Representatives\. To view the schema of the `organizations_json` table, type the following:

```
orgs = glueContext.create_dynamic_frame.from_catalog(
           database="legislators",
           table_name="organizations_json")
print "Count: ", orgs.count()
orgs.printSchema()
```

The output is as follows:

```
Count:  13
root
|-- classification: string
|-- links: array
|    |-- element: struct
|    |    |-- note: string
|    |    |-- url: string
|-- image: string
|-- identifiers: array
|    |-- element: struct
|    |    |-- scheme: string
|    |    |-- identifier: string
|-- other_names: array
|    |-- element: struct
|    |    |-- lang: string
|    |    |-- note: string
|    |    |-- name: string
|-- id: string
|-- name: string
|-- seats: int
|-- type: string
```

## Step 4: Filter the Data<a name="aws-glue-programming-python-samples-legislators-filtering"></a>

Next, keep only the fields that you want, and rename `id` to `org_id`\. The dataset is small enough that you can view the whole thing\. 

The `toDF()` converts a `DynamicFrame` to an Apache Spark `DataFrame`, so you can apply the transforms that already exist in Apache Spark SQL:

```
orgs = orgs.drop_fields(['other_names',
                        'identifiers']).rename_field(
                            'id', 'org_id').rename_field(
                               'name', 'org_name')
orgs.toDF().show()
```

The following shows the output:

```
+--------------+--------------------+--------------------+--------------------+-----+-----------+--------------------+
|classification|              org_id|            org_name|               links|seats|       type|               image|
+--------------+--------------------+--------------------+--------------------+-----+-----------+--------------------+
|         party|            party/al|                  AL|                null| null|       null|                null|
|         party|      party/democrat|            Democrat|[[website,http://...| null|       null|https://upload.wi...|
|         party|party/democrat-li...|    Democrat-Liberal|[[website,http://...| null|       null|                null|
|   legislature|d56acebe-8fdc-47b...|House of Represen...|                null|  435|lower house|                null|
|         party|   party/independent|         Independent|                null| null|       null|                null|
|         party|party/new_progres...|     New Progressive|[[website,http://...| null|       null|https://upload.wi...|
|         party|party/popular_dem...|    Popular Democrat|[[website,http://...| null|       null|                null|
|         party|    party/republican|          Republican|[[website,http://...| null|       null|https://upload.wi...|
|         party|party/republican-...|Republican-Conser...|[[website,http://...| null|       null|                null|
|         party|      party/democrat|            Democrat|[[website,http://...| null|       null|https://upload.wi...|
|         party|   party/independent|         Independent|                null| null|       null|                null|
|         party|    party/republican|          Republican|[[website,http://...| null|       null|https://upload.wi...|
|   legislature|8fa6c3d2-71dc-478...|              Senate|                null|  100|upper house|                null|
+--------------+--------------------+--------------------+--------------------+-----+-----------+--------------------+
```

Type the following to view the `organizations` that appear in `memberships`:

```
memberships.select_fields(['organization_id']).toDF().distinct().show()
```

The following shows the output:

```
+--------------------+
|     organization_id|
+--------------------+
|d56acebe-8fdc-47b...|
|8fa6c3d2-71dc-478...|
+--------------------+
```

## Step 5: Put It All Together<a name="aws-glue-programming-python-samples-legislators-joining"></a>

Now, use AWS Glue to join these relational tables and create one full history table of legislator `memberships` and their corresponding `organizations`\.

1. First, join `persons` and `memberships` on `id` and `person_id`\.

1. Next, join the result with `orgs` on `org_id` and `organization_id`\.

1. Then, drop the redundant fields, `person_id` and `org_id`\.

You can do all these operations in one \(extended\) line of code:

```
l_history = Join.apply(orgs,
                       Join.apply(persons, memberships, 'id', 'person_id'),
                       'org_id', 'organization_id').drop_fields(['person_id', 'org_id'])
print "Count: ", l_history.count()
l_history.printSchema()
```

The output is as follows:

```
Count:  10439
root
|-- role: string
|-- seats: int
|-- org_name: string
|-- links: array
|    |-- element: struct
|    |    |-- note: string
|    |    |-- url: string
|-- type: string
|-- sort_name: string
|-- area_id: string
|-- images: array
|    |-- element: struct
|    |    |-- url: string
|-- on_behalf_of_id: string
|-- other_names: array
|    |-- element: struct
|    |    |-- note: string
|    |    |-- name: string
|    |    |-- lang: string
|-- contact_details: array
|    |-- element: struct
|    |    |-- type: string
|    |    |-- value: string
|-- name: string
|-- birth_date: string
|-- organization_id: string
|-- gender: string
|-- classification: string
|-- death_date: string
|-- legislative_period_id: string
|-- identifiers: array
|    |-- element: struct
|    |    |-- scheme: string
|    |    |-- identifier: string
|-- image: string
|-- given_name: string
|-- family_name: string
|-- id: string
|-- start_date: string
|-- end_date: string
```

You now have the final table that you can use for analysis\. You can write it out in a compact, efficient format for analytics—namely Parquet—that you can run SQL over in AWS Glue, Amazon Athena, or Amazon Redshift Spectrum\.

The following call writes the table across multiple files to support fast parallel reads when doing analysis later:

```
glueContext.write_dynamic_frame.from_options(frame = l_history,
          connection_type = "s3",
          connection_options = {"path": "s3://glue-sample-target/output-dir/legislator_history"},
          format = "parquet")
```

To put all the history data into a single file, you must convert it to a data frame, repartition it, and write it out:

```
s_history = l_history.toDF().repartition(1)
s_history.write.parquet('s3://glue-sample-target/output-dir/legislator_single')
```

Or, if you want to separate it by the Senate and the House:

```
l_history.toDF().write.parquet('s3://glue-sample-target/output-dir/legislator_part',
                               partitionBy=['org_name'])
```

## Step 6: Write the Data to Relational Databases<a name="aws-glue-programming-python-samples-legislators-writing"></a>

AWS Glue makes it easy to write the data to relational databases like Amazon Redshift, even with semi\-structured data\. It offers a transform `relationalize`, which flattens `DynamicFrames` no matter how complex the objects in the frame might be\.

Using the `l_history` `DynamicFrame` in this example, pass in the name of a root table \(`hist_root`\) and a temporary working path to `relationalize`\. This returns a `DynamicFrameCollection`\. You can then list the names of the `DynamicFrames` in that collection:

```
dfc = l_history.relationalize("hist_root", "s3://glue-sample-target/temp-dir/")
dfc.keys()
```

The following is the output of the `keys` call:

```
[u'hist_root', u'hist_root_contact_details', u'hist_root_links',
 u'hist_root_other_names', u'hist_root_images', u'hist_root_identifiers']
```

`Relationalize` broke the history table out into six new tables: a root table that contains a record for each object in the `DynamicFrame`, and auxiliary tables for the arrays\. Array handling in relational databases is often suboptimal, especially as those arrays become large\. Separating the arrays into different tables makes the queries go much faster\.

Next, look at the separation by examining `contact_details`:

```
l_history.select_fields('contact_details').printSchema()
dfc.select('hist_root_contact_details').toDF().where("id = 10 or id = 75").orderBy(['id','index']).show()
```

The following is the output of the `show` call:

```
root
|-- contact_details: array
|    |-- element: struct
|    |    |-- type: string
|    |    |-- value: string
+---+-----+------------------------+-------------------------+
| id|index|contact_details.val.type|contact_details.val.value|
+---+-----+------------------------+-------------------------+
| 10|    0|                     fax|                         |
| 10|    1|                        |             202-225-1314|
| 10|    2|                   phone|                         |
| 10|    3|                        |             202-225-3772|
| 10|    4|                 twitter|                         |
| 10|    5|                        |          MikeRossUpdates|
| 75|    0|                     fax|                         |
| 75|    1|                        |             202-225-7856|
| 75|    2|                   phone|                         |
| 75|    3|                        |             202-225-2711|
| 75|    4|                 twitter|                         |
| 75|    5|                        |                SenCapito|
+---+-----+------------------------+-------------------------+
```

The `contact_details` field was an array of structs in the original `DynamicFrame`\. Each element of those arrays is a separate row in the auxiliary table, indexed by `index`\. The `id` here is a foreign key into the `hist_root` table with the key `contact_details`:

```
dfc.select('hist_root').toDF().where(
    "contact_details = 10 or contact_details = 75").select(
       ['id', 'given_name', 'family_name', 'contact_details']).show()
```

The following is the output:

```
+--------------------+----------+-----------+---------------+
|                  id|given_name|family_name|contact_details|
+--------------------+----------+-----------+---------------+
|f4fc30ee-7b42-432...|      Mike|       Ross|             10|
|e3c60f34-7d1b-4c0...|   Shelley|     Capito|             75|
+--------------------+----------+-----------+---------------+
```

Notice in these commands that `toDF()` and then a `where` expression are used to filter for the rows that you want to see\.

So, joining the `hist_root` table with the auxiliary tables lets you do the following:
+ Load data into databases without array support\.
+ Query each individual item in an array using SQL\.

You already have a connection set up named `redshift3`\. For information about how to create your own connection, see [Adding a Connection to Your Data Store](populate-add-connection.md)\. 

Next, write this collection into Amazon Redshift by cycling through the `DynamicFrames` one at a time:

```
for df_name in dfc.keys():
    m_df = dfc.select(df_name)
    print "Writing to Redshift table: ", df_name
    glueContext.write_dynamic_frame.from_jdbc_conf(frame = m_df,
                                                   catalog_connection = "redshift3",
                                                   connection_options = {"dbtable": df_name, "database": "testdb"},
                                                   redshift_tmp_dir = "s3://glue-sample-target/temp-dir/")
```

The `dbtable` property is the name of the JDBC table\. For JDBC data stores that support schemas within a database, specify `schema.table-name`\. If a schema is not provided, then the default "public" schema is used\.

For more information, see [Connection Types and Options for ETL in AWS Glue](aws-glue-programming-etl-connect.md)\.

Here's what the tables look like in Amazon Redshift\. \(You connected to Amazon Redshift through psql\.\)

```
testdb=# \d
                   List of relations
 schema |           name            | type  |   owner
--------+---------------------------+-------+-----------
 public | hist_root                 | table | test_user
 public | hist_root_contact_details | table | test_user
 public | hist_root_identifiers     | table | test_user
 public | hist_root_images          | table | test_user
 public | hist_root_links           | table | test_user
 public | hist_root_other_names     | table | test_user
(6 rows)

testdb=# \d hist_root_contact_details
             Table "public.hist_root_contact_details"
          Column           |           Type           | Modifiers
---------------------------+--------------------------+-----------
 id                        | bigint                   |
 index                     | integer                  |
 contact_details.val.type  | character varying(65535) |
 contact_details.val.value | character varying(65535) |

testdb=# \d hist_root
                   Table "public.hist_root"
        Column         |           Type           | Modifiers
-----------------------+--------------------------+-----------
 role                  | character varying(65535) |
 seats                 | integer                  |
 org_name              | character varying(65535) |
 links                 | bigint                   |
 type                  | character varying(65535) |
 sort_name             | character varying(65535) |
 area_id               | character varying(65535) |
 images                | bigint                   |
 on_behalf_of_id       | character varying(65535) |
 other_names           | bigint                   |
 birth_date            | character varying(65535) |
 name                  | character varying(65535) |
 organization_id       | character varying(65535) |
 gender                | character varying(65535) |
 classification        | character varying(65535) |
 legislative_period_id | character varying(65535) |
 identifiers           | bigint                   |
 given_name            | character varying(65535) |
 image                 | character varying(65535) |
 family_name           | character varying(65535) |
 id                    | character varying(65535) |
 death_date            | character varying(65535) |
 start_date            | character varying(65535) |
 contact_details       | bigint                   |
 end_date              | character varying(65535) |
```

Now you can query these tables using SQL in Amazon Redshift:

```
testdb=# select * from hist_root_contact_details where id = 10 or id = 75 order by id, index;
```

The following shows the result:

```
  id | index | contact_details.val.type | contact_details.val.value
  ---+-------+--------------------------+---------------------------
  10 |     0 | fax                      | 202-224-6020
  10 |     1 | phone                    | 202-224-3744
  10 |     2 | twitter                  | ChuckGrassley
  75 |     0 | fax                      | 202-224-4680
  75 |     1 | phone                    | 202-224-4642
  75 |     2 | twitter                  | SenJackReed
  (6 rows)
```

## Conclusion<a name="aws-glue-programming-python-samples-legislators-conclusion"></a>

Overall, AWS Glue is very flexible\. It lets you accomplish, in a few lines of code, what normally would take days to write\. You can find the entire source\-to\-target ETL scripts in the Python file `join_and_relationalize.py` in the [AWS Glue samples](https://github.com/awslabs/aws-glue-samples) on GitHub\.