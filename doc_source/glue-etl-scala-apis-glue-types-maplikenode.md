# AWS Glue Scala MapLikeNode APIs<a name="glue-etl-scala-apis-glue-types-maplikenode"></a>

**Package: com\.amazonaws\.services\.glue\.types**

## MapLikeNode Class<a name="glue-etl-scala-apis-glue-types-maplikenode-class"></a>

**MapLikeNode**

```
class MapLikeNode extends DynamicNode  (
           value : mutable.Map[String, DynamicNode] )
```

### MapLikeNode def Methods<a name="glue-etl-scala-apis-glue-types-maplikenode-class-defs"></a>

```
def clear : Unit 
```

```
def get( name : String ) : Option[DynamicNode] 
```

```
def getValue
```

```
def has( name : String ) : Boolean 
```

```
def isEmpty : Boolean 
```

```
def put( name : String,
         node : DynamicNode
       ) : Option[DynamicNode]
```

```
def remove( name : String ) : Option[DynamicNode] 
```

```
def toIterator : Iterator[(String, DynamicNode)] 
```

```
def toJson : String 
```

```
def toJson( useQuotes : Boolean ) : String 
```

**Example:** Given this JSON: 

```
{"foo": "bar"}
```

If `useQuotes == true`, `toJson` yields `{"foo": "bar"}`\. If `useQuotes == false`, `toJson` yields `{foo: bar}` @return\.