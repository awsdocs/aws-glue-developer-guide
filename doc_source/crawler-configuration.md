# Configuring a Crawler<a name="crawler-configuration"></a>

When a crawler runs, it might encounter changes to your data store that result in a schema or partition that is different from a previous crawl\. You can use the AWS Management Console or the AWS Glue API to configure how your crawler processes certain types of changes\. 

**Topics**
+ [Configuring a Crawler on the AWS Glue Console](#crawler-configure-changes-console)
+ [Configuring a Crawler Using the API](#crawler-configure-changes-api)
+ [How to Prevent the Crawler from Changing an Existing Schema](#crawler-schema-changes-prevent)

## Configuring a Crawler on the AWS Glue Console<a name="crawler-configure-changes-console"></a>

When you define a crawler using the AWS Glue console, you have several options for configuring the behavior of your crawler\. For more information about using the AWS Glue console to add a crawler, see [Working with Crawlers on the AWS Glue Console](console-crawlers.md)\.

When a crawler runs against a previously crawled data store, it might discover that a schema has changed or that some objects in the data store have been deleted\. The crawler logs changes to a schema\. New tables and partitions are always created regardless of the schema change policy\.

To specify what the crawler does when it finds changes in the schema, you can choose one of the following actions on the console:
+ **Update the table definition in the Data Catalog** – Add new columns, remove missing columns, and modify the definitions of existing columns in the AWS Glue Data Catalog\. Remove any metadata that is not set by the crawler\. This is the default setting\.
+ **Add new columns only** – For tables that map to an Amazon S3 data store, add new columns as they are discovered, but don't remove or change the type of existing columns in the Data Catalog\. Choose this option when the current columns in the Data Catalog are correct and you don't want the crawler to remove or change the type of the existing columns\. If a fundamental Amazon S3 table attribute changes, such as classification, compression type, or CSV delimiter, mark the table as deprecated\. Maintain input format and output format as they exist in the Data Catalog\. Update SerDe parameters only if the parameter is one that is set by the crawler\. *For all other data stores, modify existing column definitions\.*
+ **Ignore the change and don't update the table in the Data Catalog**

A crawler might also discover new or changed partitions\. By default, new partitions are added and existing partitions are updated if they have changed\. In addition, you can set a crawler configuration option to **Update all new and existing partitions with metadata from the table** on the AWS Glue console\. When this option is set, partitions inherit metadata properties—such as their classification, input format, output format, SerDe information, and schema—from their parent table\. Any changes to these properties in a table are propagated to its partitions\. When this configuration option is set on an existing crawler, existing partitions are updated to match the properties of their parent table the next time the crawler runs\. 

To specify what the crawler does when it finds a deleted object in the data store, choose one of the following actions:
+ **Delete tables and partitions from the Data Catalog**
+ **Ignore the change and don't update the table in the Data Catalog**
+ **Mark the table as deprecated in the Data Catalog** – This is the default setting\.

## Configuring a Crawler Using the API<a name="crawler-configure-changes-api"></a>

When you define a crawler using the AWS Glue API, you can choose from several fields to configure your crawler\. The `SchemaChangePolicy` in the crawler API determines what the crawler does when it discovers a changed schema or a deleted object\. The crawler logs schema changes as it runs\.

When a crawler runs, new tables and partitions are always created regardless of the schema change policy\. You can choose one of the following actions in the `UpdateBehavior` field in the `SchemaChangePolicy` structure to determine what the crawler does when it finds a changed table schema:
+ `UPDATE_IN_DATABASE` – Update the table in the AWS Glue Data Catalog\. Add new columns, remove missing columns, and modify the definitions of existing columns\. Remove any metadata that is not set by the crawler\. 
+ `LOG` – Ignore the changes, and don't update the table in the Data Catalog\.

You can also override the `SchemaChangePolicy` structure using a JSON object supplied in the crawler API `Configuration` field\. This JSON object can contain a key\-value pair to set the policy to not update existing columns and only add new columns\. For example, provide the following JSON object as a string:

```
{
   "Version": 1.0,
   "CrawlerOutput": {
      "Tables": { "AddOrUpdateBehavior": "MergeNewColumns" }
   }
}
```

This option corresponds to the **Add new columns only** option on the AWS Glue console\. It overrides the `SchemaChangePolicy` structure for tables that result from crawling Amazon S3 data stores only\. Choose this option if you want to maintain the metadata as it exists in the Data Catalog \(the source of truth\)\. New columns are added as they are encountered, including nested data types\. But existing columns are not removed, and their type is not changed\. If an Amazon S3 table attribute changes significantly, mark the table as deprecated, and log a warning that an incompatible attribute needs to be resolved\.  

When a crawler runs against a previously crawled data store, it might discover new or changed partitions\. By default, new partitions are added and existing partitions are updated if they have changed\. In addition, you can set a crawler configuration option to `InheritFromTable` \(corresponding to the **Update all new and existing partitions with metadata from the table** option on the AWS Glue console\)\. When this option is set, partitions inherit metadata properties from their parent table, such as their classification, input format, output format, SerDe information, and schema\. Any property changes to the parent table are propagated to its partitions\. 

When this configuration option is set on an existing crawler, existing partitions are updated to match the properties of their parent table the next time the crawler runs\. This behavior is set crawler API `Configuration` field\. For example, provide the following JSON object as a string: 

```
{
   "Version": 1.0,
   "CrawlerOutput": {
      "Partitions": { "AddOrUpdateBehavior": "InheritFromTable" }
   }
}
```

The crawler API `Configuration` field can set multiple configuration options\. For example, to configure the crawler output for both partitions and tables, you can provide a string representation of the following JSON object:

```
{
    "Version": 1.0,
    "CrawlerOutput": {
       "Partitions": { "AddOrUpdateBehavior": "InheritFromTable" },
       "Tables": {"AddOrUpdateBehavior": "MergeNewColumns" }
    }
}
```

You can choose one of the following actions to determine what the crawler does when it finds a deleted object in the data store\. The `DeleteBehavior` field in the `SchemaChangePolicy` structure in the crawler API sets the behavior of the crawler when it discovers a deleted object\. 
+ `DELETE_FROM_DATABASE` – Delete tables and partitions from the Data Catalog\.
+ `LOG` – Ignore the change and don't update the table in the Data Catalog\.
+ `DEPRECATE_IN_DATABASE` – Mark the table as deprecated in the Data Catalog\. This is the default setting\.

## How to Prevent the Crawler from Changing an Existing Schema<a name="crawler-schema-changes-prevent"></a>

If you don't want a crawler to overwrite updates you made to existing fields in an Amazon S3 table definition, choose the option on the console to **Add new columns only** or set the configuration option `MergeNewColumns`\. This applies to tables and partitions, unless `Partitions.AddOrUpdateBehavior` is overridden to `InheritFromTable`\.  

If you don't want a table schema to change at all when a crawler runs, set the schema change policy to `LOG`\. You can also set a configuration option that sets partition schemas to inherit from the table\. 

If you are configuring the crawler on the console, you can choose the following actions: 
+ **Ignore the change and don't update the table in the Data Catalog**
+ **Update all new and existing partitions with metadata from the table**

When you configure the crawler using the API, set the following parameters:
+ Set the `UpdateBehavior` field in `SchemaChangePolicy` structure to `LOG`\.
+  Set the `Configuration` field with a string representation of the following JSON object in the crawler API; for example: 

  ```
  {
     "Version": 1.0,
     "CrawlerOutput": {
        "Partitions": { "AddOrUpdateBehavior": "InheritFromTable" }
     }
  }
  ```