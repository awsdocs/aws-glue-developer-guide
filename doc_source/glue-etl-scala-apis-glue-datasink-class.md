# Abstract DataSink Class<a name="glue-etl-scala-apis-glue-datasink-class"></a>

**Topics**
+ [def writeDynamicFrame](#glue-etl-scala-apis-glue-datasink-class-defs-writeDynamicFrame)
+ [def pyWriteDynamicFrame](#glue-etl-scala-apis-glue-datasink-class-defs-pyWriteDynamicFrame)
+ [def supportsFormat](#glue-etl-scala-apis-glue-datasink-class-defs-supportsFormat)
+ [def setFormat](#glue-etl-scala-apis-glue-datasink-class-defs-setFormat)
+ [def withFormat](#glue-etl-scala-apis-glue-datasink-class-defs-withFormat)
+ [def setAccumulableSize](#glue-etl-scala-apis-glue-datasink-class-defs-setAccumulableSize)
+ [def getOutputErrorRecordsAccumulable](#glue-etl-scala-apis-glue-datasink-class-defs-getOutputErrorRecordsAccumulable)
+ [def errorsAsDynamicFrame](#glue-etl-scala-apis-glue-datasink-class-defs-errorsAsDynamicFrame)
+ [DataSink Object](#glue-etl-scala-apis-glue-datasink-object)

**Package: com\.amazonaws\.services\.glue**

```
abstract class DataSink
```

The writer analog to a `DataSource`\. `DataSink` encapsulates a destination and a format that a `DynamicFrame` can be written to\.

## def writeDynamicFrame<a name="glue-etl-scala-apis-glue-datasink-class-defs-writeDynamicFrame"></a>

```
def writeDynamicFrame( frame : DynamicFrame,
                       callSite : CallSite = CallSite("Not provided", "")
                     ) : DynamicFrame
```

## def pyWriteDynamicFrame<a name="glue-etl-scala-apis-glue-datasink-class-defs-pyWriteDynamicFrame"></a>

```
def pyWriteDynamicFrame( frame : DynamicFrame,
                         site : String = "Not provided",
                         info : String = "" )
```

## def supportsFormat<a name="glue-etl-scala-apis-glue-datasink-class-defs-supportsFormat"></a>

```
def supportsFormat( format : String ) : Boolean
```

## def setFormat<a name="glue-etl-scala-apis-glue-datasink-class-defs-setFormat"></a>

```
def setFormat( format : String,
               options : JsonOptions
             ) : Unit
```

## def withFormat<a name="glue-etl-scala-apis-glue-datasink-class-defs-withFormat"></a>

```
def withFormat( format : String,
                options : JsonOptions = JsonOptions.empty
              ) : DataSink
```

## def setAccumulableSize<a name="glue-etl-scala-apis-glue-datasink-class-defs-setAccumulableSize"></a>

```
def setAccumulableSize( size : Int ) : Unit
```

## def getOutputErrorRecordsAccumulable<a name="glue-etl-scala-apis-glue-datasink-class-defs-getOutputErrorRecordsAccumulable"></a>

```
def getOutputErrorRecordsAccumulable : Accumulable[List[OutputError], OutputError]
```

## def errorsAsDynamicFrame<a name="glue-etl-scala-apis-glue-datasink-class-defs-errorsAsDynamicFrame"></a>

```
def errorsAsDynamicFrame : DynamicFrame
```

## DataSink Object<a name="glue-etl-scala-apis-glue-datasink-object"></a>

```
object DataSink
```

### def recordMetrics<a name="glue-etl-scala-apis-glue-datasink-object-defs-recordMetrics"></a>

```
def recordMetrics( frame : DynamicFrame,
                   ctxt : String
                 ) : DynamicFrame
```