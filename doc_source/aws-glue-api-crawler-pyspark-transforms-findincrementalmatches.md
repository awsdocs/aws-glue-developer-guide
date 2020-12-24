# FindIncrementalMatches Class<a name="aws-glue-api-crawler-pyspark-transforms-findincrementalmatches"></a>

Identifies matching records in the existing and incremental `DynamicFrame` and creates a new `DynamicFrame` with a unique identifier assigned to each group of matching records\.

To import:

```
from awsglueml.transforms import FindIncrementalMatches
```

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-findincrementalmatches-_methods"></a>
+ [apply](#aws-glue-api-crawler-pyspark-transforms-findincrementalmatches-apply)

## apply\(existingFrame, incrementalFrame, transformId, transformation\_ctx = "", info = "", stageThreshold = 0, totalThreshold = 0, enforcedMatches = None\)<a name="aws-glue-api-crawler-pyspark-transforms-findincrementalmatches-apply"></a>

Identifies matching records in the input `DynamicFrame` and creates a new `DynamicFrame` with a unique identifier assigned to each group of matching records\.
+ `existingFrame` – The existing and pre\-matched `DynamicFrame` to apply the FindIncrementalMatches transform\. Required\.
+ `incrementalFrame` – The incremental `DynamicFrame` to apply the FindIncrementalMatches transform to match against the `existingFrame`\. Required\.
+ `transformId` – The unique ID associated with the FindIncrementalMatches transform to apply on records in the `DynamicFrames`\. Required\.
+ `transformation_ctx` – A unique string that is used to identify stats/state information\. Optional\.
+ `info` – A string to be associated with errors in the transformation\. Optional\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out\. Optional\. The default is zero\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out\. Optional\. The default is zero\.
+ `enforcedMatches` – The `DynamicFrame` used to enforce matches\. Optional\. The default is None\.

Returns a new `DynamicFrame` with a unique identifier assigned to each group of matching records\.