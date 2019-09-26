# Code Example: Data Preparation Using ResolveChoice, Lambda, and ApplyMapping<a name="aws-glue-programming-python-samples-medicaid"></a>

The dataset that is used in this example consists of Medicare Provider payment data downloaded from two `Data.CMS.gov` sites: [Inpatient Prospective Payment System Provider Summary for the Top 100 Diagnosis\-Related Groups \- FY2011](https://data.cms.gov/Medicare-Inpatient/Inpatient-Prospective-Payment-System-IPPS-Provider/97k6-zzx3)\), and [Inpatient Charge Data FY 2011](https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Provider-Charge-Data/Inpatient2011.html)\. After downloading it, we modified the data to introduce a couple of erroneous records at the end of the file\. This modified file is located in a public Amazon S3 bucket at `s3://awsglue-datasets/examples/medicare/Medicare_Hospital_Provider.csv`\.

You can find the source code for this example in the `data_cleaning_and_lambda.py` file in the [AWS Glue examples](https://github.com/awslabs/aws-glue-samples) GitHub repository\.

The easiest way to debug Python or PySpark scripts is to create a development endpoint and run your code there\. We recommend that you start by setting up a development endpoint to work in\. For more information, see [Viewing Development Endpoint Properties](console-development-endpoint.md)\.

## Step 1: Crawl the Data in the Amazon S3 Bucket<a name="aws-glue-programming-python-samples-medicaid-crawling"></a>

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. Following the process described in [Working with Crawlers on the AWS Glue Console](console-crawlers.md), create a new crawler that can crawl the `s3://awsglue-datasets/examples/medicare/Medicare_Hospital_Provider.csv` file, and can place the resulting metadata into a database named `payments` in the AWS Glue Data Catalog\.

1. Run the new crawler, and then check the `payments` database\. You should find that the crawler has created a metadata table named `medicare` in the database after reading the beginning of the file to determine its format and delimiter\.

   The schema of the new `medicare` table is as follows:

   ```
   Column  name                            Data type
   ==================================================
   drg definition                             string
   provider id                                bigint
   provider name                              string
   provider street address                    string
   provider city                              string
   provider state                             string
   provider zip code                          bigint
   hospital referral region description       string
   total discharges                           bigint
   average covered charges                    string
   average total payments                     string
   average medicare payments                  string
   ```

## Step 2: Add Boilerplate Script to the Development Endpoint Notebook<a name="aws-glue-programming-python-samples-medicaid-boilerplate"></a>

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

## Step 3: Compare Different Schema Parsings<a name="aws-glue-programming-python-samples-medicaid-schemas"></a>

Next, you can see if the schema that was recognized by an Apache Spark `DataFrame` is the same as the one that your AWS Glue crawler recorded\. Run this code:

```
medicare = spark.read.format(
   "com.databricks.spark.csv").option(
   "header", "true").option(
   "inferSchema", "true").load(
   's3://awsglue-datasets/examples/medicare/Medicare_Hospital_Provider.csv')
medicare.printSchema()
```

Here's the output from the `printSchema` call:

```
root
 |-- DRG Definition: string (nullable = true)
 |-- Provider Id: string (nullable = true)
 |-- Provider Name: string (nullable = true)
 |-- Provider Street Address: string (nullable = true)
 |-- Provider City: string (nullable = true)
 |-- Provider State: string (nullable = true)
 |-- Provider Zip Code: integer (nullable = true)
 |-- Hospital Referral Region Description: string (nullable = true)
 |--  Total Discharges : integer (nullable = true)
 |--  Average Covered Charges : string (nullable = true)
 |--  Average Total Payments : string (nullable = true)
 |-- Average Medicare Payments: string (nullable = true)
```

Next, look at the schema that an AWS Glue `DynamicFrame` generates:

```
medicare_dynamicframe = glueContext.create_dynamic_frame.from_catalog(
       database = "payments",
       table_name = "medicare")
medicare_dynamicframe.printSchema()
```

The output from `printSchema` is as follows:

```
root
 |-- drg definition: string
 |-- provider id: choice
 |    |-- long
 |    |-- string
 |-- provider name: string
 |-- provider street address: string
 |-- provider city: string
 |-- provider state: string
 |-- provider zip code: long
 |-- hospital referral region description: string
 |-- total discharges: long
 |-- average covered charges: string
 |-- average total payments: string
 |-- average medicare payments: string
```

The `DynamicFrame` generates a schema in which `provider id` could be either a `long` or a `string` type\. The `DataFrame` schema lists `Provider Id` as being a `string` type, and the Data Catalog lists `provider id` as being a `bigint` type\.

Which one is correct? There are two records at the end of the file \(out of 160,000 records\) with `string` values in that column\. These are the erroneous records that were introduced to illustrate a problem\.

To address this kind of problem, the AWS Glue `DynamicFrame` introduces the concept of a *choice* type\. In this case, the `DynamicFrame` shows that both `long` and `string` values can appear in that column\. The AWS Glue crawler missed the `string` values because it considered only a 2 MB prefix of the data\. The Apache Spark `DataFrame` considered the whole dataset, but it was forced to assign the most general type to the column, namely `string`\. In fact, Spark often resorts to the most general case when there are complex types or variations with which it is unfamiliar\.

To query the `provider id` column, resolve the choice type first\. You can use the `resolveChoice` transform method in your `DynamicFrame` to convert those `string` values to `long` values with a `cast:long` option:

```
medicare_res = medicare_dynamicframe.resolveChoice(specs = [('provider id','cast:long')])
medicare_res.printSchema()
```

The `printSchema` output is now:

```
root
 |-- drg definition: string
 |-- provider id: long
 |-- provider name: string
 |-- provider street address: string
 |-- provider city: string
 |-- provider state: string
 |-- provider zip code: long
 |-- hospital referral region description: string
 |-- total discharges: long
 |-- average covered charges: string
 |-- average total payments: string
 |-- average medicare payments: string
```

Where the value was a `string` that could not be cast, AWS Glue inserted a `null`\.

Another option is to convert the choice type to a `struct`, which keeps values of both types\.

Next, look at the rows that were anomalous:

```
medicare_res.toDF().where("'provider id' is NULL").show()
```

You see the following:

```
+--------------------+-----------+---------------+-----------------------+-------------+--------------+-----------------+------------------------------------+----------------+-----------------------+----------------------+-------------------------+
|      drg definition|provider id|  provider name|provider street address|provider city|provider state|provider zip code|hospital referral region description|total discharges|average covered charges|average total payments|average medicare payments|
+--------------------+-----------+---------------+-----------------------+-------------+--------------+-----------------+------------------------------------+----------------+-----------------------+----------------------+-------------------------+
|948 - SIGNS & SYM...|       null|            INC|       1050 DIVISION ST|      MAUSTON|            WI|            53948|                        WI - Madison|              12|              $11961.41|              $4619.00|                 $3775.33|
|948 - SIGNS & SYM...|       null| INC- ST JOSEPH|     5000 W CHAMBERS ST|    MILWAUKEE|            WI|            53210|                      WI - Milwaukee|              14|              $10514.28|              $5562.50|                 $4522.78|
+--------------------+-----------+---------------+-----------------------+-------------+--------------+-----------------+------------------------------------+----------------+-----------------------+----------------------+-------------------------+
```

Now remove the two malformed records, as follows:

```
medicare_dataframe = medicare_res.toDF()
medicare_dataframe = medicare_dataframe.where("'provider id' is NOT NULL")
```

## Step 4: Map the Data and Use Apache Spark Lambda Functions<a name="aws-glue-programming-python-samples-medicaid-lambda-mapping"></a>

AWS Glue does not yet directly support Lambda functions, also known as user\-defined functions\. But you can always convert a `DynamicFrame` to and from an Apache Spark `DataFrame` to take advantage of Spark functionality in addition to the special features of `DynamicFrames`\.

Next, turn the payment information into numbers, so analytic engines like Amazon Redshift or Amazon Athena can do their number crunching faster:

```
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

chop_f = udf(lambda x: x[1:], StringType())
medicare_dataframe = medicare_dataframe.withColumn(
        "ACC", chop_f(
            medicare_dataframe["average covered charges"])).withColumn(
                "ATP", chop_f(
                    medicare_dataframe["average total payments"])).withColumn(
                        "AMP", chop_f(
                            medicare_dataframe["average medicare payments"]))
medicare_dataframe.select(['ACC', 'ATP', 'AMP']).show()
```

The output from the `show` call is as follows:

```
+--------+-------+-------+
|     ACC|    ATP|    AMP|
+--------+-------+-------+
|32963.07|5777.24|4763.73|
|15131.85|5787.57|4976.71|
|37560.37|5434.95|4453.79|
|13998.28|5417.56|4129.16|
|31633.27|5658.33|4851.44|
|16920.79|6653.80|5374.14|
|11977.13|5834.74|4761.41|
|35841.09|8031.12|5858.50|
|28523.39|6113.38|5228.40|
|75233.38|5541.05|4386.94|
|67327.92|5461.57|4493.57|
|39607.28|5356.28|4408.20|
|22862.23|5374.65|4186.02|
|31110.85|5366.23|4376.23|
|25411.33|5282.93|4383.73|
| 9234.51|5676.55|4509.11|
|15895.85|5930.11|3972.85|
|19721.16|6192.54|5179.38|
|10710.88|4968.00|3898.88|
|51343.75|5996.00|4962.45|
+--------+-------+-------+
only showing top 20 rows
```

These are all still strings in the data\. We can use the powerful `apply_mapping` transform method to drop, rename, cast, and nest the data so that other data programming languages and systems can easily access it:

```
medicare_tmp_dyf = DynamicFrame.fromDF(medicare_dataframe, glueContext, "nested")
medicare_nest_dyf = medicare_tmp_dyf.apply_mapping([('drg definition', 'string', 'drg', 'string'),
                 ('provider id', 'long', 'provider.id', 'long'),
                 ('provider name', 'string', 'provider.name', 'string'),
                 ('provider city', 'string', 'provider.city', 'string'),
                 ('provider state', 'string', 'provider.state', 'string'),
                 ('provider zip code', 'long', 'provider.zip', 'long'),
                 ('hospital referral region description', 'string','rr', 'string'),
                 ('ACC', 'string', 'charges.covered', 'double'),
                 ('ATP', 'string', 'charges.total_pay', 'double'),
                 ('AMP', 'string', 'charges.medicare_pay', 'double')])
medicare_nest_dyf.printSchema()
```

The `printSchema` output is as follows:

```
root
 |-- drg: string
 |-- provider: struct
 |    |-- id: long
 |    |-- name: string
 |    |-- city: string
 |    |-- state: string
 |    |-- zip: long
 |-- rr: string
 |-- charges: struct
 |    |-- covered: double
 |    |-- total_pay: double
 |    |-- medicare_pay: double
```

Turning the data back into a Spark `DataFrame`, you can show what it looks like now:

```
medicare_nest_dyf.toDF().show()
```

The output is as follows:

```
+--------------------+--------------------+---------------+--------------------+
|                 drg|            provider|             rr|             charges|
+--------------------+--------------------+---------------+--------------------+
|039 - EXTRACRANIA...|[10001,SOUTHEAST ...|    AL - Dothan|[32963.07,5777.24...|
|039 - EXTRACRANIA...|[10005,MARSHALL M...|AL - Birmingham|[15131.85,5787.57...|
|039 - EXTRACRANIA...|[10006,ELIZA COFF...|AL - Birmingham|[37560.37,5434.95...|
|039 - EXTRACRANIA...|[10011,ST VINCENT...|AL - Birmingham|[13998.28,5417.56...|
|039 - EXTRACRANIA...|[10016,SHELBY BAP...|AL - Birmingham|[31633.27,5658.33...|
|039 - EXTRACRANIA...|[10023,BAPTIST ME...|AL - Montgomery|[16920.79,6653.8,...|
|039 - EXTRACRANIA...|[10029,EAST ALABA...|AL - Birmingham|[11977.13,5834.74...|
|039 - EXTRACRANIA...|[10033,UNIVERSITY...|AL - Birmingham|[35841.09,8031.12...|
|039 - EXTRACRANIA...|[10039,HUNTSVILLE...|AL - Huntsville|[28523.39,6113.38...|
|039 - EXTRACRANIA...|[10040,GADSDEN RE...|AL - Birmingham|[75233.38,5541.05...|
|039 - EXTRACRANIA...|[10046,RIVERVIEW ...|AL - Birmingham|[67327.92,5461.57...|
|039 - EXTRACRANIA...|[10055,FLOWERS HO...|    AL - Dothan|[39607.28,5356.28...|
|039 - EXTRACRANIA...|[10056,ST VINCENT...|AL - Birmingham|[22862.23,5374.65...|
|039 - EXTRACRANIA...|[10078,NORTHEAST ...|AL - Birmingham|[31110.85,5366.23...|
|039 - EXTRACRANIA...|[10083,SOUTH BALD...|    AL - Mobile|[25411.33,5282.93...|
|039 - EXTRACRANIA...|[10085,DECATUR GE...|AL - Huntsville|[9234.51,5676.55,...|
|039 - EXTRACRANIA...|[10090,PROVIDENCE...|    AL - Mobile|[15895.85,5930.11...|
|039 - EXTRACRANIA...|[10092,D C H REGI...|AL - Tuscaloosa|[19721.16,6192.54...|
|039 - EXTRACRANIA...|[10100,THOMAS HOS...|    AL - Mobile|[10710.88,4968.0,...|
|039 - EXTRACRANIA...|[10103,BAPTIST ME...|AL - Birmingham|[51343.75,5996.0,...|
+--------------------+--------------------+---------------+--------------------+
only showing top 20 rows
```

## Step 5: Write the Data to Apache Parquet<a name="aws-glue-programming-python-samples-medicaid-writing"></a>

AWS Glue makes it easy to write the data in a format such as Apache Parquet that relational databases can effectively consume:

```
glueContext.write_dynamic_frame.from_options(
       frame = medicare_nest_dyf,
       connection_type = "s3",
       connection_options = {"path": "s3://glue-sample-target/output-dir/medicare_parquet"},
       format = "parquet")
```