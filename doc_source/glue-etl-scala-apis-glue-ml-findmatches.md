# FindMatches Class<a name="glue-etl-scala-apis-glue-ml-findmatches"></a>

**Package: com\.amazonaws\.services\.glue\.ml**

```
object FindMatches
```

## def apply<a name="glue-etl-scala-apis-glue-ml-findmatches-defs-apply"></a>

```
def apply(frame: DynamicFrame,
            transformId: String,
            transformationContext: String = "",
            callSite: CallSite = CallSite("Not provided", ""),
            stageThreshold: Long = 0,
            totalThreshold: Long = 0,
            enforcedMatches: DynamicFrame = null): DynamicFrame
```

Find matches in an input frame and return a new frame with a new column containing a unique ID per match group\.
+ `frame` — The DynamicFrame in which to find matches\. Required\.
+ `transformId` — A unique ID associated with the FindMatches transform to apply on the input frame\. Required\.
+ `transformationContext` — Identifier for this `DynamicFrame`\. The `transformationContext` is used as a key for the job bookmark state that is persisted across runs\. Optional\.
+ `callSite` — Used to provide context information for error reporting\. These values are automatically set when calling from Python\. Optional\.
+ `stageThreshold` — The maximum number of error records allowed from the computation of this `DynamicFrame` before throwing an exception, excluding records present in the previous `DynamicFrame`\. Optional\. The default is zero\.
+ `totalThreshold` — The maximum number of total errors records before an exception is thrown, including those from previous frames\. Optional\. The default is zero\.
+ `enforcedMatches` — The frame for enforced matches\. Optional\. The default is `null`\.

Returns a new dynamic frame with a unique identifier assigned to each group of matching records\.