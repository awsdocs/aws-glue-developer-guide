# FillMissingValues Class<a name="glue-etl-scala-apis-glue-ml-fillmissingvalues"></a>

**Package: com\.amazonaws\.services\.glue\.ml**

```
object FillMissingValues
```

## def apply<a name="glue-etl-scala-apis-glue-ml-fillmissingvalues-defs-apply"></a>

```
def apply(frame: DynamicFrame,
          missingValuesColumn: String,
          outputColumn: String = "",
          transformationContext: String = "",
          callSite: CallSite = CallSite("Not provided", ""),
          stageThreshold: Long = 0,
          totalThreshold: Long = 0): DynamicFrame
```

Fills a dynamic frame's missing values in a specified column and returns a new frame with estimates in a new column\. For rows without missing values, the specified column's value is duplicated to the new column\.
+ `frame` — The DynamicFrame in which to fill missing values\. Required\.
+ `missingValuesColumn` — The column containing missing values \(`null` values and empty strings\)\. Required\.
+ `outputColumn` — The name of the new column that will contain estimated values for all rows whose value was missing\. Optional; the default is the value of `missingValuesColumn` suffixed by `"_filled"`\.
+ `transformationContext` — A unique string that is used to identify state information \(optional\)\.
+ `callSite` — Used to provide context information for error reporting\. \(optional\)\.
+ `stageThreshold` — The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` — The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

Returns a new dynamic frame with one additional column that contains estimations for rows with missing values and the present value for other rows\.