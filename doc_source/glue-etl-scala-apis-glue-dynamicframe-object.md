# The DynamicFrame Object<a name="glue-etl-scala-apis-glue-dynamicframe-object"></a>

**Package: com\.amazonaws\.services\.glue**

```
object DynamicFrame
```

## def apply<a name="glue-etl-scala-apis-glue-dynamicframe-object-defs-apply"></a>

```
def apply( df : DataFrame,
           glueContext : GlueContext
         ) : DynamicFrame
```

## def emptyDynamicFrame<a name="glue-etl-scala-apis-glue-dynamicframe-object-defs-emptyDynamicFrame"></a>

```
def emptyDynamicFrame( glueContext : GlueContext ) : DynamicFrame 
```

## def fromPythonRDD<a name="glue-etl-scala-apis-glue-dynamicframe-object-defs-fromPythonRDD"></a>

```
def fromPythonRDD( rdd : JavaRDD[Array[Byte]],
                   glueContext : GlueContext
                 ) : DynamicFrame
```

## def ignoreErrors<a name="glue-etl-scala-apis-glue-dynamicframe-object-defs-ignoreErrors"></a>

```
def ignoreErrors( fn : DynamicRecord => DynamicRecord ) : DynamicRecord 
```

## def inlineErrors<a name="glue-etl-scala-apis-glue-dynamicframe-object-defs-inlineErrors"></a>

```
def inlineErrors( msg : String,
                  callSite : CallSite
                ) : (DynamicRecord => DynamicRecord)
```

## def newFrameWithErrors<a name="glue-etl-scala-apis-glue-dynamicframe-object-defs-newFrameWithErrors"></a>

```
def newFrameWithErrors( prevFrame : DynamicFrame,
                        rdd : RDD[DynamicRecord],
                        name : String = "",
                        transformationContext : String = "",
                        callSite : CallSite,
                        stageThreshold : Long,
                        totalThreshold : Long
                      ) : DynamicFrame
```