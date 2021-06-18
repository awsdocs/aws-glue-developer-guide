# AWS Glue Schema Registry<a name="schema-registry"></a>

The AWS Glue Schema Registry is a new feature that allows you to centrally discover, control, and evolve data stream schemas\. A *schema* defines the structure and format of a data record\. With AWS Glue Schema Registry, you can manage and enforce schemas on your data streaming applications using convenient integrations with Apache Kafka, [Amazon Managed Streaming for Apache Kafka](https://aws.amazon.com/msk/), [Amazon Kinesis Data Streams](https://aws.amazon.com/kinesis/data-streams/), [Amazon Kinesis Data Analytics for Apache Flink](https://aws.amazon.com/kinesis/data-analytics/), and [AWS Lambda](https://aws.amazon.com/lambda/)\.

The AWS Glue Schema Registry supports AVRO \(v1\.10\.2\) data format and Java language support, with other data formats and languages to come\. Supported features include compatibility, schema sourcing via metadata, auto\-registration of schemas, IAM compatibility, and optional ZLIB compression to reduce storage and data transfer\. AWS Glue Schema Registry is serverless and free to use\.

Using a schema as a data format contract between producers and consumers leads to improved data governance, higher quality data, and enables data consumers to be resilient to compatible upstream changes\.

The Schema Registry allows disparate systems to share a schema for serialization and de\-serialization\. For example, assume you have a producer and consumer of data\. The producer knows the schema when it publishes the data\. The Schema Registry supplies a serializer and deserializer for certain systems such as Amazon MSK or Apache Kafka\.   

 For more information, see [How the Schema Registry Works](schema-registry-works.md)\.

**Topics**
+ [Schemas](#schema-registry-schemas)
+ [Registries](#schema-registry-registries)
+ [Schema Versioning and Compatibility](#schema-registry-compatibility)
+ [Open Source Serde Libraries](#schema-registry-serde-libraries)
+ [Quotas of the Schema Registry](#schema-registry-quotas)
+ [How the Schema Registry Works](schema-registry-works.md)
+ [Getting Started with Schema Registry](schema-registry-gs.md)
+ [Integrating with AWS Glue Schema Registry](schema-registry-integrations.md)

## Schemas<a name="schema-registry-schemas"></a>

A *schema* defines the structure and format of a data record\. A schema is a versioned specification for reliable data publication, consumption, or storage\.

In this example schema for Avro, the format and structure are defined by the layout and field names, and the format of the field names is defined by the data types \(e\.g\., `string`, `int`\)\.

```
{
    "type": "record",
    "namespace": "ABC_Organization",
    "name": "Employee",
    "fields": [
        {
            "name": "Name",
            "type": "string"
        },
        {
            "name": "Age",
            "type": "int"
        },
        {
            "name": "address",
            "type": {
                "type": "record",
                "name": "addressRecord",
                "fields": [
                    {
                        "name": "street",
                        "type": "string"
                    },
                    {
                        "name": "zipcode",
                        "type": "int" 
                    }
                ]
            }
        }
    ]
}
```

## Registries<a name="schema-registry-registries"></a>

A *registry* is a logical container of schemas\. Registries allow you to organize your schemas, as well as manage access control for your applications\. A registry has an Amazon Resource Name \(ARN\) to allow you to organize and set different access permissions to schema operations within the registry\.

You may use the default registry or create as many new registries as necessary\.


**AWS Glue Schema Registry Hierarchy**  

|  | 
| --- |
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/schema-registry.html)  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/schema-registry.html)  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/schema-registry.html)  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/schema-registry.html)  | 

## Schema Versioning and Compatibility<a name="schema-registry-compatibility"></a>

Each schema can have multiple versions\. Versioning is governed by a compatibility rule that is applied on a schema\. Requests to register new schema versions are checked against this rule by the Schema Registry before they can succeed\. 

A schema version that is marked as a checkpoint is used to determine the compatibility of registering new versions of a schema\. When a schema first gets created the default checkpoint will be the first version\. As the schema evolves with more versions, you can use the CLI/SDK to change the checkpoint to a version of a schema using the `UpdateSchema` API that adheres to a set of constraints\. In the console, editing the schema definition or compatibility mode will change the checkpoint to the latest version by default\. 

Compatibility modes allow you to control how schemas can or cannot evolve over time\. These modes form the contract between applications producing and consuming data\. When a new version of a schema is submitted to the registry, the compatibility rule applied to the schema name is used to determine if the new version can be accepted\. There are 8 compatibility modes: NONE, DISABLED, BACKWARD, BACKWARD\_ALL, FORWARD, FORWARD\_ALL, FULL, FULL\_ALL\.

In the Avro data format, fields may be optional or required\. An optional field is one in which the `Type` includes null\. Required fields do not have null as the `Type`\.
+ *NONE*: No compatibility mode applies\. You can use this choice in development scenarios or if you do not know the compatibility modes that you want to apply to schemas\. Any new version added will be accepted without undergoing a compatibility check\.
+ *DISABLED*: This compatibility choice prevents versioning for a particular schema\. No new versions can be added\.
+ *BACKWARD*: This compatibility choice is recommended because it allows consumers to read both the current and the previous schema version\. You can use this choice to check compatibility against the previous schema version when you delete fields or add optional fields\. A typical use case for BACKWARD is when your application has been created for the most recent schema\.

  For example, assume you have a schema defined by first name \(required\), last name \(required\), email \(required\), and phone number \(optional\)\.

  If your next schema version removes the required email field, this would successfully register\. BACKWARD compatibility requires consumers to be able to read the current and previous schema version\. Your consumers will be able to read the new schema as the extra email field from old messages is ignored\.

  If you have a proposed new schema version that adds a required field, for example, zip code, this would not successfully register with BACKWARD compatibility\. Your consumers on the new version would not be able to read old messages before the schema change, as they are missing the required zip code field\. However, if the zip code field was set as optional in the new schema, then the proposed version would successfully register as consumers can read the old schema without the optional zip code field\.
+ *BACKWARD\_ALL*: This compatibility choice allows consumers to read both the current and all previous schema versions\. You can use this choice to check compatibility against all previous schema versions when you delete fields or add optional fields\.
+ *FORWARD*: This compatibility choice allows consumers to read both the current and the subsequent schema versions, but not necessarily later versions\. You can use this choice to check compatibility against the last schema version when you add fields or delete optional fields\. A typical use case for FORWARD is when your application has been created for a previous schema and should be able to process a more recent schema\.

  For example, assume you have a schema version defined by first name \(required\), last name \(required\), email \(optional\)\.

  If you have a new schema version that adds a required field, eg\. phone number, this would successfully register\. FORWARD compatibility requires consumers to be able to read data produced with the new schema by using the previous version\.

  If you have a proposed schema version that deletes the required first name field, this would not successfully register with FORWARD compatibility\. Your consumers on the prior version would not be able to read the proposed schemas as they are missing the required first name field\. However, if the first name field was originally optional, then the proposed new schema would successfully register as the consumers can read data based on the new schema that doesn’t have the optional first name field\.
+ *FORWARD\_ALL*: This compatibility choice allows consumers to read data written by producers of any new registered schema\. You can use this choice when you need to add fields or delete optional fields, and check compatibility against all previous schema versions\.
+ *FULL*: This compatibility choice allows consumers to read data written by producers using the previous or next version of the schema, but not earlier or later versions\. You can use this choice to check compatibility against the last schema version when you add or remove optional fields\.
+ *FULL\_ALL*: This compatibility choice allows consumers to read data written by producers using all previous schema versions\. You can use this choice to check compatibility against all previous schema versions when you add or remove optional fields\.

## Open Source Serde Libraries<a name="schema-registry-serde-libraries"></a>

AWS provides open\-source Serde libraries as a framework for serializing and deserializing data\. The open source design of these libraries allows common open\-source applications and frameworks to support these libraries in their projects\.

For more details on how the Serde libraries work, see [How the Schema Registry Works](schema-registry-works.md)\.

## Quotas of the Schema Registry<a name="schema-registry-quotas"></a>

Quotas, also referred to as limits in AWS, are the maximum values for the resources, actions, and items in your AWS account\. The following are soft limits for the Schema Registry in AWS Glue\.

**Registries**  
You can have up to 10 registries per AWS account per AWS Region\.

**SchemaVersion**  
You can have up to 1000 schema versions per AWS account per AWS Region\.

Each new schema creates a new schema version, so you can theoretically have up to 1000 schemas per account per region, if each schema has only one version\.

**Schema version metadata key\-value pairs**  
You can have up to 10 key\-value pairs per SchemaVersion per AWS Region\.

You can view or set the key\-value metadata pairs using the [QuerySchemaVersionMetadata Action \(Python: query\_schema\_version\_metadata\)](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-QuerySchemaVersionMetadata) or [PutSchemaVersionMetadata Action \(Python: put\_schema\_version\_metadata\)](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-PutSchemaVersionMetadata) APIs\.