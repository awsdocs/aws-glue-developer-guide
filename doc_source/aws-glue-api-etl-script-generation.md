# AWS Glue API for Autogenerating ETL Scripts<a name="aws-glue-api-etl-script-generation"></a>

## Data Types<a name="aws-glue-api-etl-script-generation-objects"></a>

+ [CodeGenNode Structure](#aws-glue-api-etl-script-generation-CodeGenNode)

+ [CodeGenNodeArg Structure](#aws-glue-api-etl-script-generation-CodeGenNodeArg)

+ [CodeGenEdge Structure](#aws-glue-api-etl-script-generation-CodeGenEdge)

+ [Location Structure](#aws-glue-api-etl-script-generation-Location)

+ [CatalogEntry Structure](#aws-glue-api-etl-script-generation-CatalogEntry)

+ [MappingEntry Structure](#aws-glue-api-etl-script-generation-MappingEntry)

## CodeGenNode Structure<a name="aws-glue-api-etl-script-generation-CodeGenNode"></a>

Represents a node in a directed acyclic graph \(DAG\)

**Fields**

+ `Id` – String, matching the [Identifier string pattern](aws-glue-api-common.md#aws-glue-api-regex-id)\. Required\.

  A node identifier that is unique within the node's graph\.

+ `NodeType` – String\. Required\.

  The type of node this is\.

+ `Args` – An array of [CodeGenNodeArg](#aws-glue-api-etl-script-generation-CodeGenNodeArg)s\. Required\.

  Properties of the node, in the form of name\-value pairs\.

+ `LineNumber` – Number \(integer\)\.

  The line number of the node\.

## CodeGenNodeArg Structure<a name="aws-glue-api-etl-script-generation-CodeGenNodeArg"></a>

An argument or property of a node\.

**Fields**

+ `Name` – String\. Required\.

  The name of the argument or property\.

+ `Value` – String\. Required\.

  The value of the argument or property\.

+ `Param` – Boolean\.

  True if the value is used as a parameter\.

## CodeGenEdge Structure<a name="aws-glue-api-etl-script-generation-CodeGenEdge"></a>

Represents a directional edge in a directed acyclic graph \(DAG\)\.

**Fields**

+ `Source` – String, matching the [Identifier string pattern](aws-glue-api-common.md#aws-glue-api-regex-id)\. Required\.

  The ID of the node at which the edge starts\.

+ `Target` – String, matching the [Identifier string pattern](aws-glue-api-common.md#aws-glue-api-regex-id)\. Required\.

  The ID of the node at which the edge ends\.

+ `TargetParameter` – String\.

  The target of the edge\.

## Location Structure<a name="aws-glue-api-etl-script-generation-Location"></a>

The location of resources\.

**Fields**

+ `Jdbc` – An array of [CodeGenNodeArg](#aws-glue-api-etl-script-generation-CodeGenNodeArg)s\.

  A JDBC location\.

+ `S3` – An array of [CodeGenNodeArg](#aws-glue-api-etl-script-generation-CodeGenNodeArg)s\.

  An Amazon S3 location\.

## CatalogEntry Structure<a name="aws-glue-api-etl-script-generation-CatalogEntry"></a>

Specifies a table definition in the Data Catalog\.

**Fields**

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The database in which the table metadata resides\.

+ `TableName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the table in question\.

## MappingEntry Structure<a name="aws-glue-api-etl-script-generation-MappingEntry"></a>

Defines a mapping\.

**Fields**

+ `SourceTable` – String\.

  The name of the source table\.

+ `SourcePath` – String\.

  The source path\.

+ `SourceType` – String\.

  The source type\.

+ `TargetTable` – String\.

  The target table\.

+ `TargetPath` – String\.

  The target path\.

+ `TargetType` – String\.

  The target type\.

## Operations<a name="aws-glue-api-etl-script-generation-actions"></a>

+ [CreateScript Action \(Python: create\_script\)](#aws-glue-api-etl-script-generation-CreateScript)

+ [GetDataflowGraph Action \(Python: get\_dataflow\_graph\)](#aws-glue-api-etl-script-generation-GetDataflowGraph)

+ [GetMapping Action \(Python: get\_mapping\)](#aws-glue-api-etl-script-generation-GetMapping)

+ [GetPlan Action \(Python: get\_plan\)](#aws-glue-api-etl-script-generation-GetPlan)

## CreateScript Action \(Python: create\_script\)<a name="aws-glue-api-etl-script-generation-CreateScript"></a>

Transforms a directed acyclic graph \(DAG\) into code\.

**Request**

+ `DagNodes` – An array of [CodeGenNode](#aws-glue-api-etl-script-generation-CodeGenNode)s\.

  A list of the nodes in the DAG\.

+ `DagEdges` – An array of [CodeGenEdge](#aws-glue-api-etl-script-generation-CodeGenEdge)s\.

  A list of the edges in the DAG\.

+ `Language` – String \(valid values: `PYTHON` | `SCALA`\)\.

  The programming language of the resulting code from the DAG\.

**Response**

+ `PythonScript` – String\.

  The Python script generated from the DAG\.

+ `ScalaCode` – String\.

  The Scala code generated from the DAG\.

**Errors**

+ `InvalidInputException`

+ `InternalServiceException`

+ `OperationTimeoutException`

## GetDataflowGraph Action \(Python: get\_dataflow\_graph\)<a name="aws-glue-api-etl-script-generation-GetDataflowGraph"></a>

Transforms a Python script into a directed acyclic graph \(DAG\)\.

**Request**

+ `PythonScript` – String\.

  The Python script to transform\.

**Response**

+ `DagNodes` – An array of [CodeGenNode](#aws-glue-api-etl-script-generation-CodeGenNode)s\.

  A list of the nodes in the resulting DAG\.

+ `DagEdges` – An array of [CodeGenEdge](#aws-glue-api-etl-script-generation-CodeGenEdge)s\.

  A list of the edges in the resulting DAG\.

**Errors**

+ `InvalidInputException`

+ `InternalServiceException`

+ `OperationTimeoutException`

## GetMapping Action \(Python: get\_mapping\)<a name="aws-glue-api-etl-script-generation-GetMapping"></a>

Creates mappings\.

**Request**

+ `Source` – A CatalogEntry object\. Required\.

  Specifies the source table\.

+ `Sinks` – An array of [CatalogEntry](#aws-glue-api-etl-script-generation-CatalogEntry)s\.

  A list of target tables\.

+ `Location` – A Location object\.

  Parameters for the mapping\.

**Response**

+ `Mapping` – An array of [MappingEntry](#aws-glue-api-etl-script-generation-MappingEntry)s\. Required\.

  A list of mappings to the specified targets\.

**Errors**

+ `InvalidInputException`

+ `InternalServiceException`

+ `OperationTimeoutException`

+ `EntityNotFoundException`

## GetPlan Action \(Python: get\_plan\)<a name="aws-glue-api-etl-script-generation-GetPlan"></a>

Gets code to perform a specified mapping\.

**Request**

+ `Mapping` – An array of [MappingEntry](#aws-glue-api-etl-script-generation-MappingEntry)s\. Required\.

  The list of mappings from a source table to target tables\.

+ `Source` – A CatalogEntry object\. Required\.

  The source table\.

+ `Sinks` – An array of [CatalogEntry](#aws-glue-api-etl-script-generation-CatalogEntry)s\.

  The target tables\.

+ `Location` – A Location object\.

  Parameters for the mapping\.

+ `Language` – String \(valid values: `PYTHON` | `SCALA`\)\.

  The programming language of the code to perform the mapping\.

**Response**

+ `PythonScript` – String\.

  A Python script to perform the mapping\.

+ `ScalaCode` – String\.

  Scala code to perform the mapping\.

**Errors**

+ `InvalidInputException`

+ `InternalServiceException`

+ `OperationTimeoutException`