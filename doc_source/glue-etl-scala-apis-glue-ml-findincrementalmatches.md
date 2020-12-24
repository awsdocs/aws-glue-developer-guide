# FindIncrementalMatches Class<a name="glue-etl-scala-apis-glue-ml-findincrementalmatches"></a>

**Package: com\.amazonaws\.services\.glue\.ml**

```
object FindIncrementalMatches
```

## def apply<a name="glue-etl-scala-apis-glue-ml-findincrementalmatches-defs-apply"></a>

```
apply(existingFrame: DynamicFrame,
            incrementalFrame: DynamicFrame,
            transformId: String,
            transformationContext: String = "",
            callSite: CallSite = CallSite("Not provided", ""),
            stageThreshold: Long = 0,
            totalThreshold: Long = 0,
            enforcedMatches: DynamicFrame = null): DynamicFrame
```

Find matches across the existing and incremental frames and return a new frame with a column containing a unique ID per match group\.
+ `existingframe` — An existing frame which has been assigned a matching ID for each group\. Required\.
+ `incrementalframe` — An incremental frame used to find matches against the existing frame\. Required\.
+ `transformId` — A unique ID associated with the FindIncrementalMatches transform to apply on the input frames\. Required\.
+ `transformationContext` — Identifier for this `DynamicFrame`\. The `transformationContext` is used as a key for the job bookmark state that is persisted across runs\. Optional\.
+ `callSite` — Used to provide context information for error reporting\. These values are automatically set when calling from Python\. Optional\.
+ `stageThreshold` — The maximum number of error records allowed from the computation of this `DynamicFrame` before throwing an exception, excluding records present in the previous `DynamicFrame`\. Optional\. The default is zero\.
+ `totalThreshold` — The maximum number of total errors records before an exception is thrown, including those from previous frames\. Optional\. The default is zero\.
+ `enforcedMatches` — The frame for enforced matches\. Optional\. The default is `null`\.

Returns a new dynamic frame with a unique identifier assigned to each group of matching records\.