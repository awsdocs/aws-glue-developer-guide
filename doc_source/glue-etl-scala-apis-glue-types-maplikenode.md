# The AWS Glue Scala MapLikeNode APIs<a name="glue-etl-scala-apis-glue-types-maplikenode"></a>

**Package:   com\.amazonaws\.services\.glue\.types**

## The MapLikeNode Class<a name="glue-etl-scala-apis-glue-types-maplikenode-class"></a>

abstract **MapLikeNode**

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

E\.g\. given this JSON: \{"foo": "bar"\} if useQuotes == true, toJson will yield \{"foo": "bar"\} if useQuotes == fase, toJson will yield \{foo: bar\} @return