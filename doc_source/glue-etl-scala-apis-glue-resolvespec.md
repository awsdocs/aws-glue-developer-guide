# AWS Glue Scala ResolveSpec APIs<a name="glue-etl-scala-apis-glue-resolvespec"></a>

**Topics**
+ [ResolveSpec Object](#glue-etl-scala-apis-glue-resolvespec-object)
+ [ResolveSpec Case Class](#glue-etl-scala-apis-glue-resolvespec-case-class)

**Package: com\.amazonaws\.services\.glue**

## ResolveSpec Object<a name="glue-etl-scala-apis-glue-resolvespec-object"></a>

 **ResolveSpec**

```
object ResolveSpec
```

### def<a name="glue-etl-scala-apis-glue-resolvespec-object-def-apply_1"></a>

```
def apply( path : String,
           action : String
         ) : ResolveSpec
```

Creates a `ResolveSpec`\.
+ `path` — A string representation of the choice field that needs to be resolved\.
+ `action` — A resolution action\. The action can be one of the following: `Project`, `KeepAsStruct`, or `Cast`\.

Returns the `ResolveSpec`\.

### def<a name="glue-etl-scala-apis-glue-resolvespec-object-def-apply_2"></a>

```
def apply( product : Product2[String, String] ) : ResolveSpec 
```

Creates a `ResolveSpec`\.
+ `product` — `Product2` of: source path, resolution action\.

Returns the `ResolveSpec`\.

## ResolveSpec Case Class<a name="glue-etl-scala-apis-glue-resolvespec-case-class"></a>

```
case class ResolveSpec extends Product2[String, String]  (
           path : SchemaPath,
           action : String )
```

Creates a `ResolveSpec`\.
+ `path` — The `SchemaPath` of the choice field that needs to be resolved\.
+ `action` — A resolution action\. The action can be one of the following: `Project`, `KeepAsStruct`, or `Cast`\.

### ResolveSpec def Methods<a name="glue-etl-scala-apis-glue-resolvespec-case-class-defs"></a>

```
def _1 : String 
```

```
def _2 : String 
```