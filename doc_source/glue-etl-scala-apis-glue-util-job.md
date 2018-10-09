# AWS Glue Scala Job APIs<a name="glue-etl-scala-apis-glue-util-job"></a>

**Package: com\.amazonaws\.services\.glue\.util**

## Job Object<a name="glue-etl-scala-apis-glue-util-job-object"></a>

 **Job**

```
object Job
```

### Job def Methods<a name="glue-etl-scala-apis-glue-util-job-object-defs"></a>

```
def commit
```

```
def init( jobName : String,
          glueContext : GlueContext,
          args : java.util.Map[String, String] = Map[String, String]().asJava
        ) : this.type
```

```
def init( jobName : String,
          glueContext : GlueContext,
          endpoint : String,
          args : java.util.Map[String, String]
        ) : this.type
```

```
def isInitialized
```

```
def reset
```

```
def runId
```