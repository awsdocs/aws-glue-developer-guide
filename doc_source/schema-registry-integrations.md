# Integrating with AWS Glue Schema Registry<a name="schema-registry-integrations"></a>

These sections describe integrations with AWS Glue Schema Registry\.

**Topics**
+ [Use Case: Connecting Schema Registry to Amazon MSK or Apache Kafka](#schema-registry-integrations-amazon-msk)
+ [Use Case: Integrating Amazon Kinesis Data Streams with the AWS Glue Schema Registry](#schema-registry-integrations-kds)
+ [Use Case: Amazon Kinesis Data Analytics for Apache Flink](#schema-registry-integrations-kinesis-data-analytics-apache-flink)
+ [Use Case: Integration with AWS Lambda](#schema-registry-integrations-aws-lambda)
+ [Use Case: AWS Glue Data Catalog](#schema-registry-integrations-aws-glue-data-catalog)
+ [Use Case: Apache Kafka Streams](#schema-registry-integrations-apache-kafka-streams)
+ [Use Case: Apache Kafka Connect](#schema-registry-integrations-apache-kafka-connect)
+ [Migration from a Third\-Party Schema Registry to AWS Glue Schema Registry](#schema-registry-integrations-migration)
+ [Sample AWS CloudFormation Template for Schema Registry](#schema-registry-integrations-cfn)

## Use Case: Connecting Schema Registry to Amazon MSK or Apache Kafka<a name="schema-registry-integrations-amazon-msk"></a>

Let's assume you are writing data to an Apache Kafka topic, and you can follow these steps to get started\.

1. Create an Amazon MSK or Apache Kafka cluster with at least one topic\. If creating an Amazon MSK cluster, you can use the AWS management console\. Follow these instructions: https://docs\.aws\.amazon\.com/msk/latest/developerguide/getting\-started\.html\.

1. Follow the [Installing SerDe Libraries](schema-registry-gs.md#schema-registry-gs-serde) step above\.

1. To create schema registries, schemas, or schema versions, follow the instructions under the [Getting Started with Schema Registry](schema-registry-gs.md) section of this document\.

1. Start your producers and consumers to use the Schema Registry to write and read records to/from the Amazon MSK or Apache Kafka topic\. Example producer and consumer code can be found in [the ReadMe file](https://github.com/awslabs/aws-glue-schema-registry/blob/master/README.md) from the Serde libraries\. The Schema Registry library on the producer will automatically serialize the record and decorate the record with a schema version ID\.

1. If the schema of this record has been inputted, or if auto\-registration is turned on, then the schema will have been registered in the Schema Registry\.

1. The consumer reading from the Amazon MSK or Apache Kafka topic, using the AWS Glue Schema Registry library, will automatically lookup the schema from the Schema Registry\.

## Use Case: Integrating Amazon Kinesis Data Streams with the AWS Glue Schema Registry<a name="schema-registry-integrations-kds"></a>

This integration requires that you have an existing Amazon Kinesis Data Stream\. For more information, see [Getting Started with Amazon Kinesis Data Streams](https://docs.aws.amazon.com/streams/latest/dev/getting-started.html)\.

There are two ways that you can interact with data in a Kinesis Data Stream\.
+ Through the Kinesis Producer Library \(KPL\) and Kinesis Client Library \(KCL\) libraries in Java\. Multi\-language support is not provided\.
+ Through the `PutRecords`, `PutRecord`, and `GetRecords` Kinesis Data Streams APIs available in the AWS Java SDK\.

If you currently use the KPL/KCL libraries, we recommend continuing to use that method\. There are updated KCL and KPL versions with Schema Registry integrated, as shown in the examples\. Otherwise, you can use the sample code to leverage the AWS Glue Schema Registry if using the KDS APIs directly\.

Schema Registry integration is only available with KPL v0\.14\.2 or later and with KCL v2\.3 or later\.

### Interacting with Data Using the KPL/KCL Libraries<a name="schema-registry-integrations-kds-libraries"></a>

This section describes integrating Kinesis Data Streams with Schema Registry using the KPL/KCL libraries\. For more information on using KPL/KCL, see [Developing Producers Using the Amazon Kinesis Producer Library](https://docs.aws.amazon.com/streams/latest/dev/developing-producers-with-kpl.html)\.

#### Setting up the Schema Registry in KPL<a name="schema-registry-integrations-kds-libraries-kpl"></a>

1. Define the schema definition for the data, data format and schema name authored in the AWS Glue Schema Registry\.

1. Optionally configure the `GlueSchemaRegistryConfiguration` object\.

1. Pass the schema object to the `addUserRecord API`\.

   ```
   private static final String SCHEMA_DEFINITION = "{"namespace": "example.avro",\n"
   + " "type": "record",\n"
   + " "name": "User",\n"
   + " "fields": [\n"
   + " {"name": "name", "type": "string"},\n"
   + " {"name": "favorite_number", "type": ["int", "null"]},\n"
   + " {"name": "favorite_color", "type": ["string", "null"]}\n"
   + " ]\n"
   + "}";
   
   KinesisProducerConfiguration config = new KinesisProducerConfiguration();
   config.setRegion("us-west-1")
   
   //[Optional] configuration for Schema Registry.
   
   GlueSchemaRegistryConfiguration schemaRegistryConfig = 
   new GlueSchemaRegistryConfiguration("us-west-1");
   
   schemaRegistryConfig.setCompression(true);
   
   config.setGlueSchemaRegistryConfiguration(schemaRegistryConfig);
   
   ///Optional configuration ends.
   
   final KinesisProducer producer = 
         new KinesisProducer(config);
         
   final ByteBuffer data = getDataToSend();
   
   com.amazonaws.services.schemaregistry.common.Schema gsrSchema = 
       new Schema(SCHEMA_DEFINITION, DataFormat.AVRO.toString(), "demoSchema");
   
   ListenableFuture<UserRecordResult> f = producer.addUserRecord(
   config.getStreamName(), TIMESTAMP, Utils.randomExplicitHashKey(), data, gsrSchema);
   
   private static ByteBuffer getDataToSend() {
         org.apache.avro.Schema avroSchema = 
           new org.apache.avro.Schema.Parser().parse(SCHEMA_DEFINITION);
   
         GenericRecord user = new GenericData.Record(avroSchema);
         user.put("name", "Emily");
         user.put("favorite_number", 32);
         user.put("favorite_color", "green");
   
         ByteArrayOutputStream outBytes = new ByteArrayOutputStream();
         Encoder encoder = EncoderFactory.get().directBinaryEncoder(outBytes, null);
         new GenericDatumWriter<>(avroSchema).write(user, encoder);
         encoder.flush();
         return ByteBuffer.wrap(outBytes.toByteArray());
    }
   ```

#### Setting up the Kinesis Client Library<a name="schema-registry-integrations-kds-libraries-kcl"></a>

You will develop your Kinesis Client Library consumer in Java\. For more information, see [Developing a Kinesis Client Library Consumer in Java](https://docs.aws.amazon.com/streams/latest/dev/kcl2-standard-consumer-java-example.html)\.

1. Create an instance of `GlueSchemaRegistryDeserializer` by passing a `GlueSchemaRegistryConfiguration` object\.

1. Pass the `GlueSchemaRegistryDeserializer` to `retrievalConfig.glueSchemaRegistryDeserializer`\.

1. Access the schema of incoming messages by calling `kinesisClientRecord.getSchema()`\.

   ```
   GlueSchemaRegistryConfiguration schemaRegistryConfig = 
       new GlueSchemaRegistryConfiguration(this.region.toString());
       
    GlueSchemaRegistryDeserializer glueSchemaRegistryDeserializer = 
       new GlueSchemaRegistryDeserializerImpl(DefaultCredentialsProvider.builder().build(), schemaRegistryConfig);
       
    RetrievalConfig retrievalConfig = configsBuilder.retrievalConfig().retrievalSpecificConfig(new PollingConfig(streamName, kinesisClient));
    retrievalConfig.glueSchemaRegistryDeserializer(glueSchemaRegistryDeserializer);
    
     Scheduler scheduler = new Scheduler(
               configsBuilder.checkpointConfig(),
               configsBuilder.coordinatorConfig(),
               configsBuilder.leaseManagementConfig(),
               configsBuilder.lifecycleConfig(),
               configsBuilder.metricsConfig(),
               configsBuilder.processorConfig(),
               retrievalConfig
           );
           
    public void processRecords(ProcessRecordsInput processRecordsInput) {
               MDC.put(SHARD_ID_MDC_KEY, shardId);
               try {
                   log.info("Processing {} record(s)", 
                   processRecordsInput.records().size());
                   processRecordsInput.records()
                   .forEach(
                       r -> 
                           log.info("Processed record pk: {} -- Seq: {} : data {} with schema: {}", 
                           r.partitionKey(), r.sequenceNumber(), recordToAvroObj(r).toString(), r.getSchema()));
               } catch (Throwable t) {
                   log.error("Caught throwable while processing records. Aborting.");
                   Runtime.getRuntime().halt(1);
               } finally {
                   MDC.remove(SHARD_ID_MDC_KEY);
               }
    }
    
    private GenericRecord recordToAvroObj(KinesisClientRecord r) {
       byte[] data = new byte[r.data().remaining()];
       r.data().get(data, 0, data.length);
       org.apache.avro.Schema schema = new org.apache.avro.Schema.Parser().parse(r.schema().getSchemaDefinition());
       DatumReader datumReader = new GenericDatumReader<>(schema);
       
       BinaryDecoder binaryDecoder = DecoderFactory.get().binaryDecoder(data, 0, data.length, null);
       return (GenericRecord) datumReader.read(null, binaryDecoder);    
    }
   ```

### Interacting with Data Using the Kinesis Data Streams APIs<a name="schema-registry-integrations-kds-apis"></a>

This section describes integrating Kinesis Data Streams with Schema Registry using the Kinesis Data Streams APIs\.

1. Update these Maven dependencies:

   ```
   <dependencyManagement>
           <dependencies>
               <dependency>
                   <groupId>com.amazonaws</groupId>
                   <artifactId>aws-java-sdk-bom</artifactId>
                   <version>1.11.884</version>
                   <type>pom</type>
                   <scope>import</scope>
               </dependency>
           </dependencies>
       </dependencyManagement>
       
       <dependencies>
           <dependency>
               <groupId>com.amazonaws</groupId>
               <artifactId>aws-java-sdk-kinesis</artifactId>
           </dependency>
   
           <dependency>
               <groupId>software.amazon.glue</groupId>
               <artifactId>schema-registry-serde</artifactId>
               <version>1.0.0</version>
           </dependency>
   
           <dependency>
               <groupId>com.fasterxml.jackson.dataformat</groupId>
               <artifactId>jackson-dataformat-cbor</artifactId>
               <version>2.11.3</version>
           </dependency>
       </dependencies>
   ```

1. In the producer, add schema header information using the `PutRecords` or `PutRecord` API in Kinesis Data Streams\.

   ```
   //The following lines add a Schema Header to the record
           com.amazonaws.services.schemaregistry.common.Schema awsSchema =
               new com.amazonaws.services.schemaregistry.common.Schema(schemaDefinition, DataFormat.AVRO.name(),
                   schemaName);
           GlueSchemaRegistrySerializerImpl glueSchemaRegistrySerializer =
               new GlueSchemaRegistrySerializerImpl(DefaultCredentialsProvider.builder().build(), new GlueSchemaRegistryConfiguration(getConfigs()));
           byte[] recordWithSchemaHeader =
               glueSchemaRegistrySerializer.encode(streamName, awsSchema, recordAsBytes);
   ```

1. In the producer, use the `PutRecords` or `PutRecord` API to put the record into the data stream\.

1. In the consumer, remove the schema record from the header, and serialize an Avro schema record\.

   ```
   //The following lines remove Schema Header from record
           GlueSchemaRegistryDeserializerImpl glueSchemaRegistryDeserializer =
               new GlueSchemaRegistryDeserializerImpl(DefaultCredentialsProvider.builder().build(), getConfigs());
           byte[] recordWithSchemaHeaderBytes = new byte[recordWithSchemaHeader.remaining()];
           recordWithSchemaHeader.get(recordWithSchemaHeaderBytes, 0, recordWithSchemaHeaderBytes.length);
           com.amazonaws.services.schemaregistry.common.Schema awsSchema =
               glueSchemaRegistryDeserializer.getSchema(recordWithSchemaHeaderBytes);
           byte[] record = glueSchemaRegistryDeserializer.getData(recordWithSchemaHeaderBytes);
   
           //The following lines serialize an AVRO schema record
           if (DataFormat.AVRO.name().equals(awsSchema.getDataFormat())) {
               Schema avroSchema = new org.apache.avro.Schema.Parser().parse(awsSchema.getSchemaDefinition());
               Object genericRecord = convertBytesToRecord(avroSchema, record);
               System.out.println(genericRecord);
           }
   ```

### Interacting with Data Using the Kinesis Data Streams APIs<a name="schema-registry-integrations-kds-apis-reference"></a>

The following is example code for using the `PutRecords` and `GetRecords` APIs\.

```
//Full sample code
import com.amazonaws.services.schemaregistry.deserializers.GlueSchemaRegistryDeserializerImpl;
import com.amazonaws.services.schemaregistry.serializers.GlueSchemaRegistrySerializerImpl;
import com.amazonaws.services.schemaregistry.utils.AVROUtils;
import com.amazonaws.services.schemaregistry.utils.AWSSchemaRegistryConstants;
import org.apache.avro.Schema;
import org.apache.avro.generic.GenericData;
import org.apache.avro.generic.GenericDatumReader;
import org.apache.avro.generic.GenericDatumWriter;
import org.apache.avro.generic.GenericRecord;
import org.apache.avro.io.Decoder;
import org.apache.avro.io.DecoderFactory;
import org.apache.avro.io.Encoder;
import org.apache.avro.io.EncoderFactory;
import software.amazon.awssdk.auth.credentials.DefaultCredentialsProvider;
import software.amazon.awssdk.services.glue.model.DataFormat;

import java.io.ByteArrayOutputStream;
import java.io.File;
import java.io.IOException;
import java.nio.ByteBuffer;
import java.util.Collections;
import java.util.HashMap;
import java.util.Map;


public class PutAndGetExampleWithEncodedData {
    static final String regionName = "us-east-2";
    static final String streamName = "testStream1";
    static final String schemaName = "User-Topic";
    static final String AVRO_USER_SCHEMA_FILE = "src/main/resources/user.avsc";
    KinesisApi kinesisApi = new KinesisApi();

    void runSampleForPutRecord() throws IOException {
        Object testRecord = getTestRecord();
        byte[] recordAsBytes = convertRecordToBytes(testRecord);
        String schemaDefinition = AVROUtils.getInstance().getSchemaDefinition(testRecord);

        //The following lines add a Schema Header to a record
        com.amazonaws.services.schemaregistry.common.Schema awsSchema =
            new com.amazonaws.services.schemaregistry.common.Schema(schemaDefinition, DataFormat.AVRO.name(),
                schemaName);
        GlueSchemaRegistrySerializerImpl glueSchemaRegistrySerializer =
            new GlueSchemaRegistrySerializerImpl(DefaultCredentialsProvider.builder().build(), new GlueSchemaRegistryConfiguration(regionName));
        byte[] recordWithSchemaHeader =
            glueSchemaRegistrySerializer.encode(streamName, awsSchema, recordAsBytes);

        //Use PutRecords api to pass a list of records
        kinesisApi.putRecords(Collections.singletonList(recordWithSchemaHeader), streamName, regionName);

        //OR
        //Use PutRecord api to pass single record
        //kinesisApi.putRecord(recordWithSchemaHeader, streamName, regionName);
    }

    byte[] runSampleForGetRecord() throws IOException {
        ByteBuffer recordWithSchemaHeader = kinesisApi.getRecords(streamName, regionName);

        //The following lines remove the schema registry header
        GlueSchemaRegistryDeserializerImpl glueSchemaRegistryDeserializer =
            new GlueSchemaRegistryDeserializerImpl(DefaultCredentialsProvider.builder().build(), new GlueSchemaRegistryConfiguration(regionName));
        byte[] recordWithSchemaHeaderBytes = new byte[recordWithSchemaHeader.remaining()];
        recordWithSchemaHeader.get(recordWithSchemaHeaderBytes, 0, recordWithSchemaHeaderBytes.length);
        
        com.amazonaws.services.schemaregistry.common.Schema awsSchema =
            glueSchemaRegistryDeserializer.getSchema(recordWithSchemaHeaderBytes);
        
        byte[] record = glueSchemaRegistryDeserializer.getData(recordWithSchemaHeaderBytes);

        //The following lines serialize an AVRO schema record
        if (DataFormat.AVRO.name().equals(awsSchema.getDataFormat())) {
            Schema avroSchema = new org.apache.avro.Schema.Parser().parse(awsSchema.getSchemaDefinition());
            Object genericRecord = convertBytesToRecord(avroSchema, record);
            System.out.println(genericRecord);
        }

        return record;
    }

    private byte[] convertRecordToBytes(final Object record) throws IOException {
        ByteArrayOutputStream recordAsBytes = new ByteArrayOutputStream();
        Encoder encoder = EncoderFactory.get().directBinaryEncoder(recordAsBytes, null);
        GenericDatumWriter datumWriter = new GenericDatumWriter<>(AVROUtils.getInstance().getSchema(record));
        datumWriter.write(record, encoder);
        encoder.flush();
        return recordAsBytes.toByteArray();
    }

    private GenericRecord convertBytesToRecord(Schema avroSchema, byte[] record) throws IOException {
        final GenericDatumReader<GenericRecord> datumReader = new GenericDatumReader<>(avroSchema);
        Decoder decoder = DecoderFactory.get().binaryDecoder(record, null);
        GenericRecord genericRecord = datumReader.read(null, decoder);
        return genericRecord;
    }

    private Map<String, String> getMetadata() {
        Map<String, String> metadata = new HashMap<>();
        metadata.put("event-source-1", "topic1");
        metadata.put("event-source-2", "topic2");
        metadata.put("event-source-3", "topic3");
        metadata.put("event-source-4", "topic4");
        metadata.put("event-source-5", "topic5");
        return metadata;
    }

    private GlueSchemaRegistryConfiguration getConfigs() {
        GlueSchemaRegistryConfiguration configs = new GlueSchemaRegistryConfiguration(regionName);
        configs.setSchemaName(schemaName);
        configs.setAutoRegistration(true);
        configs.setMetadata(getMetadata());
        return configs;
    }

    private Object getTestRecord() throws IOException {
        GenericRecord genericRecord;
        Schema.Parser parser = new Schema.Parser();
        Schema avroSchema = parser.parse(new File(AVRO_USER_SCHEMA_FILE));

        genericRecord = new GenericData.Record(avroSchema);
        genericRecord.put("name", "testName");
        genericRecord.put("favorite_number", 99);
        genericRecord.put("favorite_color", "red");

        return genericRecord;
    }
}
```

## Use Case: Amazon Kinesis Data Analytics for Apache Flink<a name="schema-registry-integrations-kinesis-data-analytics-apache-flink"></a>

Apache Flink is a popular open source framework and distributed processing engine for stateful computations over unbounded and bounded data streams\. Amazon Kinesis Data Analytics for Apache Flink is a fully managed AWS service that enables you to build and manage Apache Flink applications to process streaming data\.

Open source Apache Flink provides a number of sources and sinks\. For example, predefined data sources include reading from files, directories, and sockets, and ingesting data from collections and iterators\. Apache Flink DataStream Connectors provide code for Apache Flink to interface with various third\-party systems, such as Apache Kafka or Kinesis as sources and/or sinks\.

For more information, see [Amazon Kinesis Data Analytics Developer Guide](https://docs.aws.amazon.com/kinesisanalytics/latest/java/what-is.html)\.

### Apache Flink Kafka Connector<a name="schema-registry-integrations-kafka-connector"></a>

Apache Flink provides an Apache Kafka data stream connector for reading data from and writing data to Kafka topics with exactly\-once guarantees\. Flink’s Kafka consumer, `FlinkKafkaConsumer`, provides access to read from one or more Kafka topics\. Apache Flink’s Kafka Producer, `FlinkKafkaProducer`, allows writing a stream of records to one or more Kafka topics\. For more information, see [Apache Kafka Connector](https://ci.apache.org/projects/flink/flink-docs-stable/dev/connectors/kafka.html)\.

### Apache Flink Kinesis Streams Connector<a name="schema-registry-integrations-kinesis-connector"></a>

The Kinesis data stream connector provides access to Amazon Kinesis Data Streams\. The `FlinkKinesisConsumer` is an exactly\-once parallel streaming data source that subscribes to multiple Kinesis streams within the same AWS service region, and can transparently handle re\-sharding of streams while the job is running\. Each subtask of the consumer is responsible for fetching data records from multiple Kinesis shards\. The number of shards fetched by each subtask will change as shards are closed and created by Kinesis\. The `FlinkKinesisProducer` uses Kinesis Producer Library \(KPL\) to put data from an Apache Flink stream into a Kinesis stream\. For more information, see [Amazon Kinesis Streams Connector](https://ci.apache.org/projects/flink/flink-docs-release-1.11/dev/connectors/kinesis.html)\.

For more information, see the [AWS Glue Schema Github repository](https://github.com/awslabs/aws-glue-schema-registry)\.

### Integrating with Apache Flink<a name="schema-registry-integrations-apache-flink-integrate"></a>

The SerDes library provided with Schema Registry integrates with Apache Flink\. To work with Apache Flink, you are required to implement [https://github.com/apache/flink/blob/master/flink-streaming-java/src/main/java/org/apache/flink/streaming/util/serialization/SerializationSchema.java](https://github.com/apache/flink/blob/master/flink-streaming-java/src/main/java/org/apache/flink/streaming/util/serialization/SerializationSchema.java) and [https://github.com/apache/flink/blob/8674b69964eae50cad024f2c5caf92a71bf21a09/flink-core/src/main/java/org/apache/flink/api/common/serialization/DeserializationSchema.java](https://github.com/apache/flink/blob/8674b69964eae50cad024f2c5caf92a71bf21a09/flink-core/src/main/java/org/apache/flink/api/common/serialization/DeserializationSchema.java) interfaces called `GlueSchemaRegistryAvroSerializationSchema` and `GlueSchemaRegistryAvroDeserializationSchema`, which you can plug into Apache Flink connectors\.

### Adding an AWS Glue Schema Registry Dependency into the Apache Flink Application<a name="schema-registry-integrations-kinesis-data-analytics-dependencies"></a>

To set up the integration dependencies to AWS Glue Schema Registry in the Apache Flink application:

1. Add the dependency to the `pom.xml` file\.

   ```
   <dependency>
       <groupId>software.amazon.glue</groupId>
       <artifactId>schema-registry-flink-serde</artifactId>
       <version>1.0.0</version>
   </dependency>
   ```

#### Integrating Kafka or MSK with Apache Flink<a name="schema-registry-integrations-kda-integrate-msk"></a>

You can use Kinesis Data Analytics for Apache Flink, with Kafka as a source or Kafka as a sink\.

**Kafka as a source**  
The following diagram shows integrating Kinesis Data Streams with Kinesis Data Analytics for Apache Flink, with Kafka as a source\.

![\[Kafka as a source.\]](http://docs.aws.amazon.com/glue/latest/dg/images/gsr-kafka-source.png)

**Kafka as a sink**  
The following diagram shows integrating Kinesis Data Streams with Kinesis Data Analytics for Apache Flink, with Kafka as a sink\.

![\[Kafka as a sink.\]](http://docs.aws.amazon.com/glue/latest/dg/images/gsr-kafka-sink.png)

To integrate Kafka \(or MSK\) with Kinesis Data Analytics for Apache Flink, with Kafka as a source or Kafka as a sink, make the code changes below\. Add the bolded code blocks to your respective code in the analogous sections\.

If Kafka is the source, then use the deserializer code \(block 2\)\. If Kafka is the sink, use the serializer code \(block 3\)\.

```
StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();

String topic = "topic";
Properties properties = new Properties();
properties.setProperty("bootstrap.servers", "localhost:9092");
properties.setProperty("group.id", "test");

// block 1
Map<String, Object> configs = new HashMap<>();
configs.put(AWSSchemaRegistryConstants.AWS_REGION, "us-east-1");
configs.put(AWSSchemaRegistryConstants.SCHEMA_AUTO_REGISTRATION_SETTING, true);
configs.put(AWSSchemaRegistryConstants.AVRO_RECORD_TYPE, AvroRecordType.GENERIC_RECORD.getName());

FlinkKafkaConsumer<GenericRecord> consumer = new FlinkKafkaConsumer<>(
    topic,
    // block 2
    GlueSchemaRegistryAvroDeserializationSchema.forGeneric(schema, configs),
    properties);

FlinkKafkaProducer<GenericRecord> producer = new FlinkKafkaProducer<>(
    topic,
    // block 3
    GlueSchemaRegistryAvroSerializationSchema.forGeneric(schema, topic, configs),
    properties);

DataStream<GenericRecord> stream = env.addSource(consumer);
stream.addSink(producer);
env.execute();
```

#### Integrating Kinesis Data Streams with Apache Flink<a name="schema-registry-integrations-integrate-kds"></a>

You can use Kinesis Data Analytics for Apache Flink with Kinesis Data Streams as a source or a sink\.

**Kinesis Data Streams as a source**  
The following diagram shows integrating Kinesis Data Streams with Kinesis Data Analytics for Apache Flink, with Kinesis Data Streams as a source\.

![\[Kinesis Data Streams as a source.\]](http://docs.aws.amazon.com/glue/latest/dg/images/gsr-kinesis-source.png)

**Kinesis Data Streams as a sink**  
The following diagram shows integrating Kinesis Data Streams with Kinesis Data Analytics for Apache Flink, with Kinesis Data Streams as a sink\.

![\[Kinesis Data Streams as a sink.\]](http://docs.aws.amazon.com/glue/latest/dg/images/gsr-kinesis-sink.png)

To integrate Kinesis Data Streams with Kinesis Data Analytics for Apache Flink, with Kinesis Data Streams as a source or Kinesis Data Streams as a sink, make the code changes below\. Add the bolded code blocks to your respective code in the analogous sections\.

If Kinesis Data Streams is the source, use the deserializer code \(block 2\)\. If Kinesis Data Streams is the sink, use the serializer code \(block 3\)\.

```
StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();

String streamName = "stream";
Properties consumerConfig = new Properties();
consumerConfig.put(AWSConfigConstants.AWS_REGION, "us-east-1");
consumerConfig.put(AWSConfigConstants.AWS_ACCESS_KEY_ID, "aws_access_key_id");
consumerConfig.put(AWSConfigConstants.AWS_SECRET_ACCESS_KEY, "aws_secret_access_key");
consumerConfig.put(ConsumerConfigConstants.STREAM_INITIAL_POSITION, "LATEST");

// block 1
Map<String, Object> configs = new HashMap<>();
configs.put(AWSSchemaRegistryConstants.AWS_REGION, "us-east-1");
configs.put(AWSSchemaRegistryConstants.SCHEMA_AUTO_REGISTRATION_SETTING, true);
configs.put(AWSSchemaRegistryConstants.AVRO_RECORD_TYPE, AvroRecordType.GENERIC_RECORD.getName());

FlinkKinesisConsumer<GenericRecord> consumer = new FlinkKinesisConsumer<>(
    streamName,
    // block 2
    GlueSchemaRegistryAvroDeserializationSchema.forGeneric(schema, configs),
    properties);

FlinkKinesisProducer<GenericRecord> producer = new FlinkKinesisProducer<>(
    // block 3
    GlueSchemaRegistryAvroSerializationSchema.forGeneric(schema, topic, configs),
    properties);
producer.setDefaultStream(streamName);
producer.setDefaultPartition("0");

DataStream<GenericRecord> stream = env.addSource(consumer);
stream.addSink(producer);
env.execute();
```

## Use Case: Integration with AWS Lambda<a name="schema-registry-integrations-aws-lambda"></a>

To use an AWS Lambda function as an Apache Kafka/Amazon MSK consumer and deserialize Avro\-encoded messages using AWS Glue Schema Registry, visit the [MSK Labs page](https://amazonmsk-labs.workshop.aws/en/msklambda/gsrschemareg.html)\.

## Use Case: AWS Glue Data Catalog<a name="schema-registry-integrations-aws-glue-data-catalog"></a>

AWS Glue tables support schemas that you can specify manually or by reference to the AWS Glue Schema Registry\. The Schema Registry integrates with the Data Catalog to allow you to optionally use schemas stored in the Schema Registry when creating or updating AWS Glue tables or partitions in the Data Catalog\. To identify a schema definition in the Schema Registry, at a minimum, you need to know the ARN of the schema it is part of\. A schema version of a schema, which contains a schema definition, can be referenced by its UUID or version number\. There is always one schema version, the "latest" version, that can be looked up without knowing its version number or UUID\.

When calling the `CreateTable` or `UpdateTable` operations, you will pass a `TableInput` structure that contains a `StorageDescriptor`, which may have a `SchemaReference` to an existing schema in the Schema Registry\. Similarly, when you call the `GetTable` or `GetPartition` APIs, the response may contain the schema and the `SchemaReference`\. When a table or partition was created using a schema references, the Data Catalog will try to fetch the schema for this schema reference\. In case it is unable to find the schema in the Schema Registry, it returns an empty schema in the `GetTable` response; otherwise the response will have both the schema and schema reference\.

You can also perform the actions from the AWS Glue console\.

To perform these operations and create, update, or view the schema information, you must give the caller IAM user permissions for the `GetSchemaVersion` API\.

### Adding a Table or Updating the Schema for a Table<a name="schema-registry-integrations-aws-glue-data-catalog-table"></a>

Adding a new table from an existing schema binds the table to a specific schema version\. Once new schema versions get registered, you can update this table definition from the View table page in the AWS Glue console or using the [UpdateTable Action \(Python: update\_table\)](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-UpdateTable) API\.

#### Adding a Table from an Existing Schema<a name="schema-registry-integrations-aws-glue-data-catalog-table-existing"></a>

You can create an AWS Glue table from a schema version in the registry using the AWS Glue console or `CreateTable` API\.

**AWS Glue API**  
When calling the `CreateTable` API, you will pass a `TableInput` that contains a `StorageDescriptor` which has a `SchemaReference` to an existing schema in the Schema Registry\.

**AWS Glue console**  
To create a table from the AWS Glue console:

1. Sign in to the AWS Management Console and open the AWS Glue console at https://console\.aws\.amazon\.com/glue/\.

1. In the navigation pane, under **Data catalog**, choose **Tables**\.

1. In the **Add Tables** menu, choose **Add table from existing schema**\.

1. Configure the table properties and data store per the AWS Glue Developer Guide\.

1. In the **Choose a Glue schema** page, select the **Registry** where the schema resides\.

1. Choose the **Schema name** and select the **Version** of the schema to apply\.

1. Review the schema preview, and choose **Next**\.

1. Review and create the table\.

The schema and version applied to the table appears in the **Glue schema** column in the list of tables\. You can view the table to see more details\.

#### Updating the Schema for a Table<a name="schema-registry-integrations-aws-glue-data-catalog-table-updating"></a>

When a new schema version becomes available, you may want to update a table's schema using the [UpdateTable Action \(Python: update\_table\)](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-UpdateTable) API or the AWS Glue console\. 

**Important**  
When updating the schema for an existing table that has an AWS Glue schema specified manually, the new schema referenced in the Schema Registry may be incompatible\. This can result in your jobs failing\.

**AWS Glue API**  
When calling the `UpdateTable` API, you will pass a `TableInput` that contains a `StorageDescriptor` which has a `SchemaReference` to an existing schema in the Schema Registry\.

**AWS Glue console**  
To update the schema for a table from the AWS Glue console:

1. Sign in to the AWS Management Console and open the AWS Glue console at https://console\.aws\.amazon\.com/glue/\.

1. In the navigation pane, under **Data catalog**, choose **Tables**\.

1. View the table from the list of tables\.

1. Click **Update schema** in the box that informs you about a new version\.

1. Review the differences between the current and new schema\.

1. Choose **Show all schema differences** to see more details\.

1. Choose **Save table** to accept the new version\.

## Use Case: Apache Kafka Streams<a name="schema-registry-integrations-apache-kafka-streams"></a>

The Apache Kafka Streams API is a client library for processing and analyzing data stored in Apache Kafka\. This section describes the integration of Apache Kafka Streams with AWS Glue Schema Registry, which allows you to manage and enforce schemas on your data streaming applications\. For more information on Apache Kafka Streams, see [Apache Kafka Streams](https://kafka.apache.org/documentation/streams/)\.

### Integrating with the SerDes Libraries<a name="schema-registry-integrations-apache-kafka-streams-integrate"></a>

There is an `AWSKafkaAvroSerDe` class that you can configure a Streams application with\.

### Kafka Streams Application Example Code<a name="schema-registry-integrations-apache-kafka-streams-application"></a>

To use the AWS Glue Schema Registry within an Apache Kafka Streams application:

1. Configure the Kafka Streams application\.

   ```
   final Properties props = new Properties();
       props.put(StreamsConfig.APPLICATION_ID_CONFIG, "avro-streams");
       props.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
       props.put(StreamsConfig.CACHE_MAX_BYTES_BUFFERING_CONFIG, 0);
       props.put(StreamsConfig.DEFAULT_KEY_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());
       props.put(StreamsConfig.DEFAULT_VALUE_SERDE_CLASS_CONFIG, AWSKafkaAvroSerDe.class.getName());
       props.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");
           
       props.put(AWSSchemaRegistryConstants.AWS_REGION, "us-east-1");
       props.put(AWSSchemaRegistryConstants.SCHEMA_AUTO_REGISTRATION_SETTING, true);
       props.put(AWSSchemaRegistryConstants.AVRO_RECORD_TYPE, AvroRecordType.GENERIC_RECORD.getName());
   ```

1. Create a stream from the topic avro\-input\.

   ```
   StreamsBuilder builder = new StreamsBuilder();
   final KStream<String, GenericRecord> source = builder.stream("avro-input");
   ```

1. Process the data records \(the example filters out those records whose value of favorite\_color is pink or where the value of amount is 15\)\.

   ```
   final KStream<String, GenericRecord> result = source
       .filter((key, value) -> !"pink".equals(String.valueOf(value.get("favorite_color"))));
       .filter((key, value) -> !"15.0".equals(String.valueOf(value.get("amount"))));
   ```

1. Write the results back to the topic avro\-output\.

   ```
   result.to("avro-output");
   ```

1. Start the Apache Kafka Streams application\.

   ```
   KafkaStreams streams = new KafkaStreams(builder.build(), props);
   streams.start();
   ```

### Implementation Results<a name="schema-registry-integrations-apache-kafka-streams-results"></a>

These results show the filtering process of records that were filtered out in step 3 as a favorite\_color of "pink" or value of "15\.0"\.

Records before filtering:

```
{"name": "Sansa", "favorite_number": 99, "favorite_color": "white"}
{"name": "Harry", "favorite_number": 10, "favorite_color": "black"}
{"name": "Hermione", "favorite_number": 1, "favorite_color": "red"}
{"name": "Ron", "favorite_number": 0, "favorite_color": "pink"}
{"name": "Jay", "favorite_number": 0, "favorite_color": "pink"}
	
{"id": "commute_1","amount": 3.5}
{"id": "grocery_1","amount": 25.5}
{"id": "entertainment_1","amount": 19.2}
{"id": "entertainment_2","amount": 105}
	{"id": "commute_1","amount": 15}
```

Records after filtering:

```
{"name": "Sansa", "favorite_number": 99, "favorite_color": "white"}
{"name": "Harry", "favorite_number": 10, "favorite_color": "black"}
{"name": "Hermione", "favorite_number": 1, "favorite_color": "red"}
{"name": "Ron", "favorite_number": 0, "favorite_color": "pink"}
	
{"id": "commute_1","amount": 3.5}
{"id": "grocery_1","amount": 25.5}
{"id": "entertainment_1","amount": 19.2}
{"id": "entertainment_2","amount": 105}
```

## Use Case: Apache Kafka Connect<a name="schema-registry-integrations-apache-kafka-connect"></a>

The integration of Apache Kafka Connect with the AWS Glue Schema Registry enables you to get schema information from connectors\. The Apache Kafka converters specify the format of data within Apache Kafka and how to translate it into Apache Kafka Connect data\. Every Apache Kafka Connect user will need to configure these converters based on the format they want their data in when loaded from or stored into Apache Kafka\. In this way, you can define your own converters to translate Apache Kafka Connect data into the type used in the AWS Glue Schema Registry \(for example: Avro\) and utilize our serializer to register its schema and do serialization\. Then converters are also able to use our deserializer to deserialize data received from Apache Kafka and convert it back into Apache Kafka Connect data\. An example workflow diagram is given below\.

![\[Apache Kafka Connect workflow.\]](http://docs.aws.amazon.com/glue/latest/dg/images/schema_reg_int_kafka_connect.png)

1. Install the `aws-glue-schema-registry` project by cloning the [Github repository for the AWS Glue Schema Registry](https://github.com/awslabs/aws-glue-schema-registry)\.

   ```
   git clone git@github.com:awslabs/aws-glue-schema-registry.git
   cd aws-glue-schema-registry
   mvn clean install
   mvn dependency:copy-dependencies
   ```

1. If you plan on using Apache Kafka Connect in *Standalone* mode, update **connect\-standalone\.properties** using the instructions below for this step\. If you plan on using Apache Kafka Connect in *Distributed* mode, update **connect\-avro\-distributed\.properties** using the same instructions\.

   1. Add these properties also to the Apache Kafka connect properties file:

      ```
      key.converter.region=us-east-2
      value.converter.region=us-east-2
      key.converter.schemaAutoRegistrationEnabled=true
      value.converter.schemaAutoRegistrationEnabled=true
      key.converter.avroRecordType=GENERIC_RECORD
      value.converter.avroRecordType=GENERIC_RECORD
      ```

   1. Add the command below to the **Launch mode** section under **kafka\-run\-class\.sh**:

      ```
      -cp $CLASSPATH:"<your AWS Glue Schema Registry base directory>/target/dependency/*"
      ```

1. Add the command below to the **Launch mode** section under **kafka\-run\-class\.sh**

   ```
   -cp $CLASSPATH:"<your AWS Glue Schema Registry base directory>/target/dependency/*" 
   ```

   It should look like this:

   ```
   # Launch mode
   if [ "x$DAEMON_MODE" = "xtrue" ]; then
     nohup "$JAVA" $KAFKA_HEAP_OPTS $KAFKA_JVM_PERFORMANCE_OPTS $KAFKA_GC_LOG_OPTS $KAFKA_JMX_OPTS $KAFKA_LOG4J_OPTS -cp $CLASSPATH:"/Users/johndoe/aws-glue-schema-registry/target/dependency/*" $KAFKA_OPTS "$@" > "$CONSOLE_OUTPUT_FILE" 2>&1 < /dev/null &
   else
     exec "$JAVA" $KAFKA_HEAP_OPTS $KAFKA_JVM_PERFORMANCE_OPTS $KAFKA_GC_LOG_OPTS $KAFKA_JMX_OPTS $KAFKA_LOG4J_OPTS -cp $CLASSPATH:"/Users/johndoe/aws-glue-schema-registry/target/dependency/*" $KAFKA_OPTS "$@"
   fi
   ```

1. If using bash, run the below commands to set\-up your CLASSPATH in your bash\_profile\. For any other shell, update the environment accordingly\.

   ```
   echo 'export GSR_LIB_BASE_DIR=<>' >>~/.bash_profile
   echo 'export GSR_LIB_VERSION=1.0.0' >>~/.bash_profile
   echo 'export KAFKA_HOME=<your Apache Kafka installation directory>' >>~/.bash_profile
   echo 'export CLASSPATH=$CLASSPATH:$GSR_LIB_BASE_DIR/avro-kafkaconnect-converter/target/schema-registry-kafkaconnect-converter-$GSR_LIB_VERSION.jar:$GSR_LIB_BASE_DIR/common/target/schema-registry-common-$GSR_LIB_VERSION.jar:$GSR_LIB_BASE_DIR/avro-serializer-deserializer/target/schema-registry-serde-$GSR_LIB_VERSION.jar' >>~/.bash_profile
   source ~/.bash_profile
   ```

1. \(Optional\) If you want to test with a simple file source, then clone the file source connector\.

   ```
   git clone https://github.com/mmolimar/kafka-connect-fs.git
   cd kafka-connect-fs/
   ```

   1. Under the source connector configuration, edit the data format to Avro, file reader to `AvroFileReader` and update an example Avro object from the file path you are reading from\. For example:

      ```
      vim config/kafka-connect-fs.properties
      ```

      ```
      fs.uris=<path to a sample avro object>
      policy.regexp=^.*\.avro$
      file_reader.class=com.github.mmolimar.kafka.connect.fs.file.reader.AvroFileReader
      ```

   1. Install the source connector\.

      ```
      mvn clean package
      echo "export CLASSPATH=\$CLASSPATH:\"\$(find target/ -type f -name '*.jar'| grep '\-package' | tr '\n' ':')\"" >>~/.bash_profile
      source ~/.bash_profile
      ```

   1. Update the sink properties under `<your Apache Kafka installation directory>/config/connect-file-sink.properties` update the topic name and out file name\.

      ```
      file=<output file full path>
      topics=<my topic>
      ```

1. Start the Source Connector \(in this example it is a file source connector\)\.

   ```
   $KAFKA_HOME/bin/connect-standalone.sh $KAFKA_HOME/config/connect-standalone.properties config/kafka-connect-fs.properties
   ```

1. Run the Sink Connector \(in this example it is a file sink connector\)\.

   ```
   $KAFKA_HOME/bin/connect-standalone.sh $KAFKA_HOME/config/connect-standalone.properties $KAFKA_HOME/config/connect-file-sink.properties
   ```

## Migration from a Third\-Party Schema Registry to AWS Glue Schema Registry<a name="schema-registry-integrations-migration"></a>

The migration from a third\-party schema registry to the AWS Glue Schema Registry has a dependency on the existing, current third\-party schema registry\. If there are records in an Apache Kafka topic which were sent using a third\-party schema registry, consumers need the third\-party schema registry to deserialize those records\. The `AWSKafkaAvroDeserializer` provides the ability to specify a secondary deserializer class which points to the third\-party deserializer and is used to deserialize those records\.

There are two criteria for retirement of a third\-party schema\. First, retirement can occur only after records in Apache Kafka topics using the 3rd party schema registry are either no longer required by and for any consumers\. Second, retirement can occur by aging out of the Apache Kafka topics, depending on the retention period specified for those topics\. Note that if you have topics which have infinite retention, you can still migrate to the AWS Glue Schema Registry but you will not be able to retire the third\-party schema registry\. As a workaround, you can use an application or Mirror Maker 2 to read from the current topic and produce to a new topic with the AWS Glue Schema Registry\.

To migrate from a third\-party schema registry to the AWS Glue Schema Registry:

1. Create a registry in the AWS Glue Schema Registry, or use the default registry\.

1. Stop the consumer\. Modify it to include AWS Glue Schema Registry as the primary deserializer, and the third\-party schema registry as the secondary\. 
   + Set the consumer properties\. In this example, the secondary\_deserializer is set to a different deserializer\. The behavior is as follows: the consumer retrieves records from Amazon MSK and first tries to use the `AWSKafkaAvroDeserializer`\. If it is unable to read the magic byte that contains the Avro Schema ID for the AWS Glue Schema Registry schema, the `AWSKafkaAvroDeserializer` then tries to use the deserializer class provided in the secondary\_deserializer\. The properties specific to the secondary deserializer also need to be provided in the consumer properties, such as the schema\_registry\_url\_config and specific\_avro\_reader\_config, as shown below\.

     ```
     consumerProps.setProperty(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
     consumerProps.setProperty(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, AWSKafkaAvroDeserializer.class.getName());
     consumerProps.setProperty(AWSSchemaRegistryConstants.AWS_REGION, KafkaClickstreamConsumer.gsrRegion);
     consumerProps.setProperty(AWSSchemaRegistryConstants.SECONDARY_DESERIALIZER, KafkaAvroDeserializer.class.getName());
     consumerProps.setProperty(KafkaAvroDeserializerConfig.SCHEMA_REGISTRY_URL_CONFIG, "URL for third-party schema registry");
     consumerProps.setProperty(KafkaAvroDeserializerConfig.SPECIFIC_AVRO_READER_CONFIG, "true");
     ```

1. Restart the consumer\.

1. Stop the producer and point the producer to the AWS Glue Schema Registry\.

   1. Set the producer properties\. In this example, the producer will use the default\-registry and auto register schema versions\.

      ```
      producerProps.setProperty(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
      producerProps.setProperty(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, AWSKafkaAvroSerializer.class.getName());
      producerProps.setProperty(AWSSchemaRegistryConstants.AWS_REGION, "us-east-2");
      producerProps.setProperty(AWSSchemaRegistryConstants.AVRO_RECORD_TYPE, AvroRecordType.SPECIFIC_RECORD.getName());
      producerProps.setProperty(AWSSchemaRegistryConstants.SCHEMA_AUTO_REGISTRATION_SETTING, "true");
      ```

1. \(Optional\) Manually move existing schemas and schema versions from the current third\-party schema registry to the AWS Glue Schema Registry, either to the default\-registry in AWS Glue Schema Registry or to a specific non\-default registry in AWS Glue Schema Registry\. This can be done by exporting schemas from the third\-party schema registries in JSON format and creating new schemas in AWS Glue Schema Registry using the AWS console or the AWS CLI\.

    This step may be important if you need to enable compatibility checks with previous schema versions for newly created schema versions using the AWS CLI and the AWS console, or when producers send messages with a new schema with auto\-registration of schema versions turned on\.

1. Start the producer\.

## Sample AWS CloudFormation Template for Schema Registry<a name="schema-registry-integrations-cfn"></a>

The following is a sample template for creating Schema Registry resources in AWS CloudFormation\. To create this stack in your account, copy the above template into a file `SampleTemplate.yaml`, and run the following command:

```
aws cloudformation create-stack --stack-name ABCSchemaRegistryStack --template-body "'cat SampleTemplate.yaml'"
```

This example uses `AWS::Glue::Registry` to create a registry, `AWS::Glue::Schema` to create a schema, `AWS::Glue::SchemaVersion` to create a schema version, and `AWS::Glue::SchemaVersionMetadata` to populate schema version metadata\. 

```
Description: "A sample CloudFormation template for creating Schema Registry resources."
Resources:
  ABCRegistry:
    Type: "AWS::Glue::Registry"
    Properties:
      Name: "ABCSchemaRegistry"
      Description: "ABC Corp. Schema Registry"
      Tags:
        - Key: "Project"
          Value: "Foo"
  ABCSchema:
    Type: "AWS::Glue::Schema"
    Properties:
      Registry:
        Arn: !Ref ABCRegistry
      Name: "TestSchema"
      Compatibility: "NONE"
      DataFormat: "AVRO"
      SchemaDefinition: >
        {"namespace":"foo.avro","type":"record","name":"user","fields":[{"name":"name","type":"string"},{"name":"favorite_number","type":"int"}]}
      Tags:
        - Key: "Project"
          Value: "Foo"
  SecondSchemaVersion:
    Type: "AWS::Glue::SchemaVersion"
    Properties:
      Schema:
        SchemaArn: !Ref ABCSchema
      SchemaDefinition: >
        {"namespace":"foo.avro","type":"record","name":"user","fields":[{"name":"status","type":"string", "default":"ON"}, {"name":"name","type":"string"},{"name":"favorite_number","type":"int"}]}
  FirstSchemaVersionMetadata:
    Type: "AWS::Glue::SchemaVersionMetadata"
    Properties:
      SchemaVersionId: !GetAtt ABCSchema.InitialSchemaVersionId
      Key: "Application"
      Value: "Kinesis"
  SecondSchemaVersionMetadata:
    Type: "AWS::Glue::SchemaVersionMetadata"
    Properties:
      SchemaVersionId: !Ref SecondSchemaVersion
      Key: "Application"
      Value: "Kinesis"
```