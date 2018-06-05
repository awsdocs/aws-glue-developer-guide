# AWS Glue Scala DataSource Trait<a name="glue-etl-scala-apis-glue-datasource-trait"></a>

**Package: com\.amazonaws\.services\.glue**

A high\-level interface for producing a `DynamicFrame`\.

```
trait DataSource {

  def getDynamicFrame : DynamicFrame 

  def getDynamicFrame( minPartitions : Int,
                       targetPartitions : Int
                     ) : DynamicFrame 

  def glueContext : GlueContext

  def setFormat( format : String,
                 options : String
               ) : Unit 

  def setFormat( format : String,
                 options : JsonOptions
               ) : Unit

  def supportsFormat( format : String ) : Boolean

  def withFormat( format : String,
                  options : JsonOptions = JsonOptions.empty
                ) : DataSource 
}
```