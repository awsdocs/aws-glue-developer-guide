# Filter Class<a name="aws-glue-api-crawler-pyspark-transforms-filter"></a>

Builds a new `DynamicFrame` by selecting records from the input `DynamicFrame` that satisfy a specified predicate function\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-filter-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-filter-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-filter-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-filter-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-filter-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-filter-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-filter-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-filter-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-filter-describe)
+ [Example Code](#aws-glue-api-crawler-pyspark-transforms-filter-example)

## \_\_call\_\_\(frame, f, transformation\_ctx="", info="", stageThreshold=0, totalThreshold=0\)\)<a name="aws-glue-api-crawler-pyspark-transforms-filter-__call__"></a>

Returns a new `DynamicFrame` built by selecting records from the input `DynamicFrame` that satisfy a specified predicate function\.
+ `frame` – The source `DynamicFrame` to apply the specified filter function to \(required\)\.
+ `f` – The predicate function to apply to each `DynamicRecord` in the `DynamicFrame`\. The function must take a `DynamicRecord` as its argument and return True if the `DynamicRecord` meets the filter requirements, or False if it does not \(required\)\.

  A `DynamicRecord` represents a logical record in a `DynamicFrame`\. It is similar to a row in a Spark `DataFrame`, except that it is self\-describing and can be used for data that does not conform to a fixed schema\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-filter-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-filter-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-filter-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-filter-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-filter-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-filter-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-filter-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.

## AWS Glue Python Example<a name="aws-glue-api-crawler-pyspark-transforms-filter-example"></a>

This example filters sample data using the `Filter` transform and a simple Lambda function\. The dataset used here consists of Medicare Provider payment data downloaded from two `Data.CMS.gov` sites: [Inpatient Prospective Payment System Provider Summary for the Top 100 Diagnosis\-Related Groups \- FY2011](https://data.cms.gov/Medicare-Inpatient/Inpatient-Prospective-Payment-System-IPPS-Provider/97k6-zzx3)\), and [Inpatient Charge Data FY 2011](https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Provider-Charge-Data/Inpatient2011.html)\. 

After downloading the sample data, we modified it to introduce a couple of erroneous records at the end of the file\. This modified file is located in a public Amazon S3 bucket at `s3://awsglue-datasets/examples/medicare/Medicare_Hospital_Provider.csv`\. For another example that uses this dataset, see [Code Example: Data Preparation Using ResolveChoice, Lambda, and ApplyMapping](aws-glue-programming-python-samples-medicaid.md)\. 

Begin by creating a `DynamicFrame` for the data:

```
%pyspark
from awsglue.context import GlueContext
from awsglue.transforms import *
from pyspark.context import SparkContext

glueContext = GlueContext(SparkContext.getOrCreate())

dyF = glueContext.create_dynamic_frame.from_options(
        's3',
        {'paths': ['s3://awsglue-datasets/examples/medicare/Medicare_Hospital_Provider.csv']},
        'csv',
        {'withHeader': True})

print "Full record count:  ", dyF.count()
dyF.printSchema()
```

The output should be as follows:

```
Full record count:  163065L
root
|-- DRG Definition: string
|-- Provider Id: string
|-- Provider Name: string
|-- Provider Street Address: string
|-- Provider City: string
|-- Provider State: string
|-- Provider Zip Code: string
|-- Hospital Referral Region Description: string
|-- Total Discharges: string
|-- Average Covered Charges: string
|-- Average Total Payments: string
|-- Average Medicare Payments: string
```

Next, use the `Filter` transform to condense the dataset, retaining only those entries that are from Sacramento, California, or from Montgomery, Alabama\. The filter transform works with any filter function that takes a `DynamicRecord` as input and returns True if the `DynamicRecord` meets the filter requirements, or False if not\.

**Note**  
You can use Python’s dot notation to access many fields in a `DynamicRecord`\. For example, you can access the `column_A` field in `dynamic_record_X` as: `dynamic_record_X.column_A`\.  
However, this technique doesn't work with field names that contain anything besides alphanumeric characters and underscores\. For fields that contain other characters, such as spaces or periods, you must fall back to Python's dictionary notation\. For example, to access a field named `col-B`, use: `dynamic_record_X["col-B"]`\.

You can use a simple Lambda function with the `Filter` transform to remove all `DynamicRecords` that don't originate in Sacramento or Montgomery\. To confirm that this worked, print out the number of records that remain:

```
sac_or_mon_dyF = Filter.apply(frame = dyF,
                              f = lambda x: x["Provider State"] in ["CA", "AL"] and x["Provider City"] in ["SACRAMENTO", "MONTGOMERY"])
print "Filtered record count:  ", sac_or_mon_dyF.count()
```

The output that you get looks like the following:

`Filtered record count: 564L`