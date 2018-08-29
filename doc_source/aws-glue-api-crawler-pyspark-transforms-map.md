# Map Class<a name="aws-glue-api-crawler-pyspark-transforms-map"></a>

Builds a new `DynamicFrame` by applying a function to all records in the input `DynamicFrame`\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-map-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-map-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-map-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-map-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-map-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-map-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-map-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-map-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-map-describe)
+ [Example Code](#aws-glue-api-crawler-pyspark-transforms-map-example)

## \_\_call\_\_\(frame, f, transformation\_ctx="", info="", stageThreshold=0, totalThreshold=0\)<a name="aws-glue-api-crawler-pyspark-transforms-map-__call__"></a>

Returns a new `DynamicFrame` that results from applying the specified function to all `DynamicRecords` in the original `DynamicFrame`\.
+ `frame` – The original `DynamicFrame` to which to apply the mapping function \(required\)\.
+ `f` – The function to apply to all `DynamicRecords` in the `DynamicFrame`\. The function must take a `DynamicRecord` as an argument and return a new `DynamicRecord` produced by the mapping \(required\)\.

  A `DynamicRecord` represents a logical record in a `DynamicFrame`\. It is similar to a row in an Apache Spark `DataFrame`, except that it is self\-describing and can be used for data that does not conform to a fixed schema\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

Returns a new `DynamicFrame` that results from applying the specified function to all `DynamicRecords` in the original `DynamicFrame`\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-map-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-map-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-map-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-map-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-map-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-map-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-map-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.

## AWS Glue Python Example<a name="aws-glue-api-crawler-pyspark-transforms-map-example"></a>

This example uses the `Map` transform to merge several fields into one `struct` type\. The dataset that is used here consists of Medicare Provider payment data downloaded from two `Data.CMS.gov` sites: [Inpatient Prospective Payment System Provider Summary for the Top 100 Diagnosis\-Related Groups \- FY2011](https://data.cms.gov/Medicare-Inpatient/Inpatient-Prospective-Payment-System-IPPS-Provider/97k6-zzx3)\), and [Inpatient Charge Data FY 2011](https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Provider-Charge-Data/Inpatient2011.html)\. 

After downloading the sample data, we modified it to introduce a couple of erroneous records at the end of the file\. This modified file is located in a public Amazon S3 bucket at `s3://awsglue-datasets/examples/medicare/Medicare_Hospital_Provider.csv`\. For another example that uses this dataset, see [Code Example: Data Preparation Using ResolveChoice, Lambda, and ApplyMapping](aws-glue-programming-python-samples-medicaid.md)\.

Begin by creating a `DynamicFrame` for the data:

```
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

The output of this code should be as follows:

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

Next, create a mapping function to merge provider\-address fields in a `DynamicRecord` into a `struct`, and then delete the individual address fields:

```
def MergeAddress(rec):
  rec["Address"] = {}
  rec["Address"]["Street"] = rec["Provider Street Address"]
  rec["Address"]["City"] = rec["Provider City"]
  rec["Address"]["State"] = rec["Provider State"]
  rec["Address"]["Zip.Code"] = rec["Provider Zip Code"]
  rec["Address"]["Array"] = [rec["Provider Street Address"], rec["Provider City"], rec["Provider State"], rec["Provider Zip Code"]]
  del rec["Provider Street Address"]
  del rec["Provider City"]
  del rec["Provider State"]
  del rec["Provider Zip Code"]
  return rec
```

In this mapping function, the line `rec["Address"] = {}` creates a dictionary in the input `DynamicRecord` that contains the new structure\.

**Note**  
Python `map` fields are *not* supported here\. For example, you can't have a line like the following:  
`rec["Addresses"] = [] # ILLEGAL!`

The lines that are like `rec["Address"]["Street"] = rec["Provider Street Address"]` add fields to the new structure using Python dictionary syntax\.

After the address lines are added to the new structure, the lines that are like `del rec["Provider Street Address"]` remove the individual fields from the `DynamicRecord`\.

Now you can use the `Map` transform to apply your mapping function to all `DynamicRecords` in the `DynamicFrame`\.

```
mapped_dyF =  Map.apply(frame = dyF, f = MergeAddress)
mapped_dyF.printSchema()
```

The output is as follows:

```
root
|-- Average Total Payments: string
|-- Average Covered Charges: string
|-- DRG Definition: string
|-- Average Medicare Payments: string
|-- Hospital Referral Region Description: string
|-- Address: struct
| |-- Zip.Code: string
| |-- City: string
| |-- Array: array
| | |-- element: string
| |-- State: string
| |-- Street: string
|-- Provider Id: string
|-- Total Discharges: string
|-- Provider Name: string
```