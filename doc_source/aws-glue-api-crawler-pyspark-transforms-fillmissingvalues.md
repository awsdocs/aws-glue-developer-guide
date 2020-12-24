# FillMissingValues Class<a name="aws-glue-api-crawler-pyspark-transforms-fillmissingvalues"></a>

Fills null values and empty strings in a specified `DynamicFrame` column using machine learning\.

To import:

```
from awsglueml.transforms import FillMissingValues
```

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-fillmissingvalues-_methods"></a>
+ [apply](#aws-glue-api-crawler-pyspark-transforms-fillmissingvalues-apply)

## apply\(frame, missing\_values\_column, output\_column ="", transformation\_ctx ="", info ="", stageThreshold = 0, totalThreshold = 0\)<a name="aws-glue-api-crawler-pyspark-transforms-fillmissingvalues-apply"></a>

Fills a dynamic frame's missing values in a specified column and returns a new frame with estimates in a new column\. For rows without missing values, the specified column's value is duplicated to the new column\.
+ `frame` – The DynamicFrame in which to fill missing values\. Required\.
+ `missing_values_column` – The column containing missing values \(`null` values and empty strings\)\. Required\.
+ `output_column` – The name of the new column that will contain estimated values for all rows whose value was missing\. Optional; the default is the name of `missing_values_column` suffixed by `"_filled"`\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

Returns a new `DynamicFrame` with one additional column that contains estimations for rows with missing values and the present value for other rows\.