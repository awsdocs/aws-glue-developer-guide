# Getting Started with Schema Registry<a name="schema-registry-gs"></a>

The following sections provide an overview and walk you through setting up and using Schema Registry\. For information about Schema Registry concepts and components, see [AWS Glue Schema Registry](schema-registry.md)\.

**Topics**
+ [Installing SerDe Libraries](#schema-registry-gs-serde)
+ [Using AWS CLI for the AWS Glue Schema Registry APIs](#schema-registry-gs-cli)
+ [Creating a Registry](#schema-registry-gs3)
+ [Creating a Schema](#schema-registry-gs4)
+ [Updating a Schema or Registry](#schema-registry-gs5)
+ [Deleting a Schema or Registry](#schema-registry-gs7)
+ [IAM Examples for Serializers](#schema-registry-gs1)
+ [IAM Examples for Deserializers](#schema-registry-gs1b)
+ [Private Connectivity using AWS PrivateLink](#schema-registry-gs-private)
+ [Accessing Amazon CloudWatch Metrics](#schema-registry-gs-monitoring)

## Installing SerDe Libraries<a name="schema-registry-gs-serde"></a>

**Note**  
Prerequisites: Before completing the following steps, you will need to have a Amazon Managed Streaming for Apache Kafka \(Amazon MSK\) or Apache Kafka cluster running\. Your producers and consumers need to be running on Java 8 or above\.

The SerDe libraries provide a framework for serializing and deserializing data\. 

You will install the open source serializer for your applications producing data \(collectively the "serializers"\)\. The serializer handles serialization, compression, and the interaction with the Schema Registry\. The serializer automatically extracts the schema from a record being written to a Schema Registry compatible destination, such as Amazon MSK\. Likewise, you will install the open source deserializer on your applications consuming data\.

To install the libraries on producers and consumers:

1. Inside both the producers’ and consumers’ pom\.xml files, add this dependency via the code below:

   ```
   <dependency>
       <groupId>software.amazon.glue</groupId>
       <artifactId>schema-registry-serde</artifactId>
       <version>1.1.5</version>
   </dependency>
   ```

   Alternatively, you can clone the [AWS Glue Schema Registry Github repository](https://github.com/awslabs/aws-glue-schema-registry)\.

1. Setup your producers with these required properties:

   ```
   props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName()); // Can replace StringSerializer.class.getName()) with any other key serializer that you may use
   props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, AWSKafkaAvroSerializer.class.getName()); 
   props.put(AWSSchemaRegistryConstants.AWS_REGION, "us-east-2");
   ```

   If there are no existing schemas, then auto\-registration needs to be turned on \(next step\)\. If you do have a schema that you would like to apply, then replace "my\-schema" with your schema name\. Also the "registry\-name" has to be provided if schema auto\-registration is off\. If the schema is created under the "default\-registry" then registry name can be omitted\.

1. \(Optional\) Set any of these optional producer properties\. For detailed property descriptions, see [the ReadMe file](https://github.com/awslabs/aws-glue-schema-registry/blob/master/README.md)\.

   ```
   props.put(AWSSchemaRegistryConstants.SCHEMA_AUTO_REGISTRATION_SETTING, "true"); // If not passed, uses "false"
   props.put(AWSSchemaRegistryConstants.SCHEMA_NAME, "my-schema"); // If not passed, uses transport name (topic name in case of Kafka, or stream name in case of Kinesis Data Streams)
   props.put(AWSSchemaRegistryConstants.REGISTRY_NAME, "my-registry"); // If not passed, uses "default-registry"
   props.put(AWSSchemaRegistryConstants.CACHE_TIME_TO_LIVE_MILLIS, "86400000"); // If not passed, uses 86400000 (24 Hours)
   props.put(AWSSchemaRegistryConstants.CACHE_SIZE, "10"); // default value is 200
   props.put(AWSSchemaRegistryConstants.COMPATIBILITY_SETTING, Compatibility.FULL); // Pass a compatibility mode. If not passed, uses Compatibility.BACKWARD
   props.put(AWSSchemaRegistryConstants.DESCRIPTION, "This registry is used for several purposes."); // If not passed, constructs a description
   props.put(AWSSchemaRegistryConstants.COMPRESSION_TYPE, AWSSchemaRegistryConstants.COMPRESSION.ZLIB); // If not passed, records are sent uncompressed
   ```

   Auto\-registration registers the schema version under the default registry \("default\-registry"\)\. If a `SCHEMA_NAME` is not specified in the previous step, then the topic name is inferred as `SCHEMA_NAME`\. 

   See [Schema Versioning and Compatibility](schema-registry.md#schema-registry-compatibility) for more information on compatibility modes\.

1. Setup your consumers with these required properties:

   ```
   props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
   props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, AWSKafkaAvroDeserializer.class.getName());
   props.put(AWSSchemaRegistryConstants.AWS_REGION, "us-east-2"); // Pass an AWS Region
   props.put(AWSSchemaRegistryConstants.AVRO_RECORD_TYPE, AvroRecordType.GENERIC_RECORD.getName());
   ```

1. \(Optional\) Set these optional consumer properties\. For detailed property descriptions, see [the ReadMe file](https://github.com/awslabs/aws-glue-schema-registry/blob/master/README.md)\.

   ```
   properties.put(AWSSchemaRegistryConstants.CACHE_TIME_TO_LIVE_MILLIS, "86400000"); // If not passed, uses 86400000
   props.put(AWSSchemaRegistryConstants.CACHE_SIZE, "10"); // default value is 200
   props.put(AWSSchemaRegistryConstants.SECONDARY_DESERIALIZER, "com.amazonaws.services.schemaregistry.deserializers.external.ThirdPartyDeserializer"); // For migration fall back scenario
   ```

## Using AWS CLI for the AWS Glue Schema Registry APIs<a name="schema-registry-gs-cli"></a>

To use the AWS CLI for the AWS Glue Schema Registry APIs, make sure to update your AWS CLI to the latest version\.

## Creating a Registry<a name="schema-registry-gs3"></a>

You may use the default registry or create as many new registries as necessary using the AWS Glue APIs or AWS Glue console\.

**AWS Glue APIs**  
You can use these steps to perform this task using the AWS Glue APIs\.

To add a new registry, use the [CreateRegistry Action \(Python: create\_registry\)](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-CreateRegistry) API\. Specify `RegistryName` as the name of the registry to be created, with a max length of 255, containing only letters, numbers, hyphens, underscores, dollar signs, or hash marks\. 

Specify a `Description` as a string not more than 2048 bytes long, matching the [URI address multi\-line string pattern](https://docs.aws.amazon.com/glue/latest/dg/latest/dg/aws-glue-api-common.html#aws-glue-api-regex-uri)\.

Optionally, specify one or more `Tags` for your registry, as a map array of key\-value pairs\.

```
aws glue create-registry --registry-name registryName1 --description description
```

When your registry is created it is assigned an Amazon Resource Name \(ARN\), which you can view in the `RegistryArn` of the API response\. Now that you've created a registry, create one or more schemas for that registry\.

**AWS Glue console**  
To add a new registry in the AWS Glue console:

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue\)\.

1. In the navigation pane, under **Data catalog**, choose **Schema registries**\.

1. Choose **Add registry**\.

1. Enter a **Registry name** for the registry, consisting of letters, numbers, hyphens, or underscores\. This name cannot be changed\.

1. Enter a **Description** \(optional\) for the registry\.

1. Optionally, apply one or more tags to your registry\. Choose **Add new tag** and specify a **Tag key** and optionally a **Tag value**\.

1. Choose **Add registry**\.

![\[Example of a creating a registry.\]](http://docs.aws.amazon.com/glue/latest/dg/images/schema_reg_create_registry.png)

When your registry is created it is assigned an Amazon Resource Name \(ARN\), which you can view by choosing the registry from the list in **Schema registries**\. Now that you've created a registry, create one or more schemas for that registry\.

## Creating a Schema<a name="schema-registry-gs4"></a>

You can create a schema using the AWS Glue APIs or the AWS Glue console\.

**AWS Glue APIs**  
You can use these steps to perform this task using the AWS Glue APIs\.

To add a new schema, use the [CreateSchema Action \(Python: create\_schema\)](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-CreateSchema) API\.

Specify a `RegistryId` structure to indicate a registry for the schema\. Or, omit the `RegistryId` to use the default registry\.

Specify a `SchemaName` consisting of letters, numbers, hyphens, or underscores, and `DataFormat` as **AVRO**\.

Specify a `Compatibility` mode:
+ *Backward \(recommended\)* — Consumer can read both current and previous version\.
+ *Backward all* — Consumer can read current and all previous versions\.
+ *Forward* — Consumer can read both current and subsequent version\.
+ *Forward all* — Consumer can read both current and all subsequent versions\.
+ *Full* — Combination of Backward and Forward\.
+ *Full all* — Combination of Backward all and Forward all\.
+ *None* — No compatibility checks are performed\.
+ *Disabled* — Prevent any versioning for this schema\.

Optionally, specify `Tags` for your schema\. 

Specify a `SchemaDefinition` to define the schema in Avro data format\. See the examples\.

```
aws glue create-schema --registry-id RegistryName="registryName1" --schema-name testschema --compatibility NONE --data-format AVRO --schema-definition "{\"type\": \"record\", \"name\": \"r1\", \"fields\": [ {\"name\": \"f1\", \"type\": \"int\"}, {\"name\": \"f2\", \"type\": \"string\"} ]}"
```

```
aws glue create-schema --registry-id RegistryArn="arn:aws:glue:us-east-2:901234567890:registry/registryName1" --schema-name testschema --compatibility NONE --data-format AVRO  --schema-definition "{\"type\": \"record\", \"name\": \"r1\", \"fields\": [ {\"name\": \"f1\", \"type\": \"int\"}, {\"name\": \"f2\", \"type\": \"string\"} ]}"
```

**AWS Glue console**  
To add a new schema using the AWS Glue console:

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue\)\.

1. In the navigation pane, under **Data catalog**, choose **Schemas**\.

1. Choose **Add schema**\.

1. Enter a **Schema name**, consisting of letters, numbers, hyphens, underscores, dollar signs, or hashmarks\. This name cannot be changed\.

1. Choose the **Registry** where the schema will be stored from the drop\-down menu\. The parent registry cannot be changed post\-creation\.

1. Leave the **Data format** as *Apache Avro*\. This format applies to all versions of this schema\.

1. Choose a **Compatibility mode**\.
   + *Backward \(recommended\)* — receiver can read both current and previous versions\.
   + *Backward All* — receiver can read current and all previous versions\.
   + *Forward* — sender can write both current and previous versions\.
   + *Forward All* — sender can write current and all previous versions\.
   + *Full* — combination of Backward and Forward\.
   + *Full All* — combination of Backward All and Forward All\.
   + *None* — no compatibility checks performed\.
   + *Disabled* — prevent any versioning for this schema\.

1. Enter an optional **Description** for the registry of up to 250 characters\.  
![\[Example of a creating a schema.\]](http://docs.aws.amazon.com/glue/latest/dg/images/schema_reg_create_schema.png)

1. Optionally, apply one or more tags to your schema\. Choose **Add new tag** and specify a **Tag key** and optionally a **Tag value**\.

1. In the **First schema version** box, enter or paste your initial Apache Avro schema\. See [Working with Avro Data Format](#schema-registry-avro)\.

1. Optionally, choose **Add metadata** to add version metadata to annotate or classify your schema version\.

1. Choose **Create schema and version**\.

![\[Example of a creating a schema.\]](http://docs.aws.amazon.com/glue/latest/dg/images/schema_reg_create_schema2.png)

The schema is created and appears in the list under **Schemas**\.

### Working with Avro Data Format<a name="schema-registry-avro"></a>

Avro provides data serialization and data exchange services\. Avro stores the data definition in JSON format making it easy to read and interpret\. The data itself is stored in binary format\.

For information on defining an Apache Avro schema, see the [Apache Avro specification](http://avro.apache.org/docs/current/spec.html)\.

## Updating a Schema or Registry<a name="schema-registry-gs5"></a>

Once created you can edit your schemas, schema versions, or registry\.

### Updating a Registry<a name="schema-registry-gs5a"></a>

You can update a registry using the AWS Glue APIs or the AWS Glue console\. The name of an existing registry cannot be edited\. You can edit the description  for a registry\.

**AWS Glue APIs**  
To update an existing registry, use the [UpdateRegistry Action \(Python: update\_registry\)](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-UpdateRegistry) API\.

Specify a `RegistryId` structure to indicate the registry that you want to update\. Pass a `Description` to change the description for a registry\.

```
aws glue update-registry --description updatedDescription --registry-id RegistryArn="arn:aws:glue:us-east-2:901234567890:registry/registryName1"
```

**AWS Glue console**  
To update a registry using the AWS Glue console:

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue\)\.

1. In the navigation pane, under **Data catalog**, choose **Schema registries**\.

1. Choose a registry from the the list of registries, by checking its box\.

1. In the **Action** menu, choose **Edit registry**\.

### Updating a Schema<a name="schema-registry-gs5b"></a>

You can update the description or compatibility setting for a schema\.

To update an existing schema, use the [UpdateSchema Action \(Python: update\_schema\)](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-UpdateSchema) API\.

Specify a `SchemaId` structure to indicate the schema that you want to update\. One of `VersionNumber` or `Compatibility` has to be provided\.

Code example 11:

```
aws glue update-schema --description testDescription --schema-id SchemaName="testSchema1",RegistryName="registryName1" --schema-version-number LatestVersion=true --compatibility NONE
```

```
aws glue update-schema --description testDescription --schema-id SchemaArn="arn:aws:glue:us-east-2:901234567890:schema/registryName1/testSchema1" --schema-version-number LatestVersion=true --compatibility NONE
```

### Adding a Schema Version<a name="schema-registry-gs5c"></a>

When you add a schema version, you will need to compare the versions to make sure the new schema will be accepted\.

To add a new version to an existing schema, use the [RegisterSchemaVersion Action \(Python: register\_schema\_version\)](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-RegisterSchemaVersion) API\.

Specify a `SchemaId` structure to indicate the schema for which you want to add a version, and a `SchemaDefinition` to define the schema in Avro data format\.

Code example 12:

```
aws glue register-schema-version --schema-definition "{\"type\": \"record\", \"name\": \"r1\", \"fields\": [ {\"name\": \"f1\", \"type\": \"int\"}, {\"name\": \"f2\", \"type\": \"string\"} ]}" --schema-id SchemaArn="arn:aws:glue:us-east-1:901234567890:schema/registryName/testschema"
```

```
aws glue register-schema-version --schema-definition "{\"type\": \"record\", \"name\": \"r1\", \"fields\": [ {\"name\": \"f1\", \"type\": \"int\"}, {\"name\": \"f2\", \"type\": \"string\"} ]}" --schema-id SchemaName="testschema",RegistryName="testregistry"
```

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue\)\.

1. In the navigation pane, under **Data catalog**, choose **Schemas**\.

1. Choose the schema from the the list of schemas, by checking its box\.

1. Choose one or more schemas from the list, by checking the boxes\.

1. In the **Action** menu, choose **Register new version**\.

1. In the **New version** box, enter or paste your new Apache Avro schema\.

1. Choose **Compare with previous version** to see differences with the previous schema version\.

1. Optionally, choose **Add metadata** to add version metadata to annotate or classify your schema version\. Enter **Key** and optional **Value**\.

1. Choose **Register version**\.

![\[Adding a schema version.\]](http://docs.aws.amazon.com/glue/latest/dg/images/schema_reg_add_schema_version.png)

The schema\(s\) version appears in the list of versions\. If the version changed the compatibility mode, the version will be marked as a checkpoint\.

#### Example of a Schema Version Comparison<a name="schema-registry-gs5c1"></a>

When you choose to **Compare with previous version**, you will see the previous and new versions displayed together\. Changed information will be highlighted as follows:
+ *Yellow*: indicates changed information\.
+ *Green*: indicates content added in the latest version\.
+ *Red*: indicates content removed in the latest version\.

You can also compare against earlier versions\.

![\[Example of a schema version comparison.\]](http://docs.aws.amazon.com/glue/latest/dg/images/schema_reg_version_comparison.png)

## Deleting a Schema or Registry<a name="schema-registry-gs7"></a>

Deleting a schema, a schema version, or a registry are permanent actions that cannot be undone\.

### Deleting a Schema<a name="schema-registry-gs7a"></a>

You may want to delete a schema when it will no longer be used within a registry, using the AWS Management Console, or the [DeleteSchema Action \(Python: delete\_schema\)](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-DeleteSchema) API\.

Deleting one or more schemas is a permanent action that cannot be undone\. Make sure that the schema or schemas are no longer needed\.

To delete a schema from the registry, call the [DeleteSchema Action \(Python: delete\_schema\)](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-DeleteSchema) API, specifying the `SchemaId` structure to identify the schema\.

For example:

```
aws glue delete-schema --schema-id SchemaArn="arn:aws:glue:us-east-2:901234567890:schema/registryName1/schemaname"
```

```
aws glue delete-schema --schema-id SchemaName="TestSchema6-deleteschemabyname",RegistryName="default-registry"
```

**AWS Glue console**  
To delete a schema from the AWS Glue console:

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue\)\.

1. In the navigation pane, under **Data catalog**, choose **Schema registries**\.

1. Choose the registry that contains your schema from the the list of registries\.

1. Choose one or more schemas from the list, by checking the boxes\.

1. In the **Action** menu, choose **Delete schema**\.

1. Enter the text **Delete** in the field to confirm deletion\.

1. Choose **Delete**\.

The schema\(s\) you specified are deleted from the registry\.

### Deleting a Schema Version<a name="schema-registry-gs7b"></a>

As schemas accumulate in the registry, you may want to delete unwanted schema versions using the AWS Management Console, or the [DeleteSchemaVersions Action \(Python: delete\_schema\_versions\)](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-DeleteSchemaVersions) API\. Deleting one or more schema versions is a permanent action that cannot be undone\. Make sure that the schema versions are no longer needed\.

When deleting schema versions, take note of the following constraints:
+ You cannot delete a check\-pointed version\.
+ The range of contiguous versions cannot be more than 25\.
+ The latest schema version must not be in a pending state\.

Specify the `SchemaId` structure to identify the schema, and specify `Versions` as a range of versions to delete\. For more information on specifying a version or range of versions, see [DeleteRegistry Action \(Python: delete\_registry\)](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-DeleteRegistry)\. The schema versions you specified are deleted from the registry\.

Calling the [ListSchemaVersions Action \(Python: list\_schema\_versions\)](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-ListSchemaVersions) API after this call will list the status of the deleted versions\.

For example:

```
aws glue delete-schema-versions --schema-id SchemaName="TestSchema6",RegistryName="default-registry" --versions "1-1"
```

```
aws glue delete-schema-versions --schema-id SchemaArn="arn:aws:glue:us-east-2:901234567890:schema/default-registry/TestSchema6-NON-Existent" --versions "1-1"
```

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue\)\.

1. In the navigation pane, under **Data catalog**, choose **Schema registries**\.

1. Choose the registry that contains your schema from the the list of registries\.

1. Choose one or more schemas from the list, by checking the boxes\.

1. In the **Action** menu, choose **Delete schema**\.

1. Enter the text **Delete** in the field to confirm deletion\.

1. Choose **Delete**\.

The schema versions you specified are deleted from the registry\.

### Deleting a Registry<a name="schema-registry-gs7c"></a>

You may want to delete a registry when the schemas it contains should no longer be organized under that registry\. You will need to reassign those schemas to another registry\.

Deleting one or more registries is a permanent action that cannot be undone\. Make sure that the registry or registries no longer needed\.

The default registry can be deleted using the AWS CLI\.

**AWS Glue API**  
To delete the entire registry including the schema and all of its versions, call the [DeleteRegistry Action \(Python: delete\_registry\)](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-DeleteRegistry) API\. Specify a `RegistryId` structure to identify the registry\.

For example:

```
aws glue delete-registry --registry-id RegistryArn="arn:aws:glue:us-east-2:901234567890:registry/registryName1"
```

```
aws glue delete-registry --registry-id RegistryName="TestRegistry-deletebyname"
```

To get the status of the delete operation, you can call the `GetRegistry` API after the asynchronous call\.

**AWS Glue console**  
To delete a registry from the AWS Glue console:

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue\)\.

1. In the navigation pane, under **Data catalog**, choose **Schema registries**\.

1. Choose a registry from the list, by checking a box\.

1. In the **Action** menu, choose **Delete registry**\.

1. Enter the text **Delete** in the field to confirm deletion\.

1. Choose **Delete**\.

The registries you selected are deleted from AWS Glue\.

## IAM Examples for Serializers<a name="schema-registry-gs1"></a>

**Note**  
AWS managed policies grant necessary permissions for common use cases\. For information on using managed policies to manage the schema registry, see [AWS Managed \(Predefined\) Policies for AWS Glue](using-identity-based-policies.md#access-policy-examples-aws-managed)\. 

For serializers, you should create a minimal policy similar to that below to give you the ability to find the `schemaVersionId` for a given schema definition\. Note, you should have read permissions on the registry in order to read the schemas in the registry\. You can limit the registries that can be read by using the `Resource` clause\.

Code example 13:

```
{
    "Sid" : "GetSchemaByDefinition",
    "Effect" : "Allow",
    "Action" : 
	[
        "glue:GetSchemaByDefinition"
    ],
        "Resource" : ["arn:aws:glue:us-east-2:012345678:registry/registryname-1",
                      "arn:aws:glue:us-east-2:012345678:schema/registryname-1/schemaname-1",
                      "arn:aws:glue:us-east-2:012345678:schema/registryname-1/schemaname-2"
                     ]  
}
```

Further, you can also allow producers to create new schemas and versions by including the following extra methods\. Note, you should be able to inspect the registry in order to add/remove/evolve the schemas inside it\. You can limit the registries that can be inspected by using the `Resource` clause\.

Code example 14:

```
{
    "Sid" : "RegisterSchemaWithMetadata",
    "Effect" : "Allow",
    "Action" : 
	[
        "glue:GetSchemaByDefinition",
        "glue:CreateSchema",
        "glue:RegisterSchemaVersion",
        "glue:PutSchemaVersionMetadata",
    ],
    "Resource" : ["arn:aws:glue:aws-region:123456789012:registry/registryname-1",
                  "arn:aws:glue:aws-region:123456789012:schema/registryname-1/schemaname-1",
                  "arn:aws:glue:aws-region:123456789012:schema/registryname-1/schemaname-2"
                 ]  
}
```

## IAM Examples for Deserializers<a name="schema-registry-gs1b"></a>

For deserializers \(consumer side\), you should create a policy similar to that below to allow the deserializer to fetch the schema from the Schema Registry for deserialization\. Note, you should be able to inspect the registry in order to fetch the schemas inside it\. You can limit the registries that can read by using the `Resource` clause\. But if access to all registries is required then it can be achieved by specifying "\*" for the appropriate portions of the ARN\.

Code example 15:

```
{
    "Sid" : "GetSchemaVersion",
    "Effect" : "Allow",
    "Action" : 
	[
        "glue:GetSchemaVersion"
    ],
    "Resource" : ["*"]  
}
```

## Private Connectivity using AWS PrivateLink<a name="schema-registry-gs-private"></a>

You can use AWS PrivateLink to connect your data producer’s VPC to AWS Glue by defining an interface VPC endpoint for AWS Glue\. When you use a VPC interface endpoint, communication between your VPC and AWS Glue is conducted entirely within the AWS network\. For more information, see [Using AWS Glue with VPC Endpoints](https://docs.aws.amazon.com/glue/latest/dg/vpc-endpoint.html)\.

## Accessing Amazon CloudWatch Metrics<a name="schema-registry-gs-monitoring"></a>

Amazon CloudWatch metrics are available as part of CloudWatch’s free tier\. You can access these metrics in the CloudWatch Console\. API\-Level metrics include CreateSchema \(Success and Latency\), GetSchemaByDefinition, \(Success and Latency\), GetSchemaVersion \(Success and Latency\), RegisterSchemaVersion \(Success and Latency\), PutSchemaVersionMetadata \(Success and Latency\)\. Resource\-level metrics include Registry\.ThrottledByLimit, SchemaVersion\.ThrottledByLimit, SchemaVersion\.Size\.
