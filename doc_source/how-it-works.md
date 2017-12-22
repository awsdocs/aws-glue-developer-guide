# AWS Glue: How It Works<a name="how-it-works"></a>

AWS Glue uses other AWS services to orchestrate your extract, transform, and load \(ETL\) jobs to build a data warehouse\. AWS Glue calls API operations to transform your data, create runtime logs, store your job logic, and create notifications to help you monitor your job runs\. The AWS Glue console connects these services into a managed application, so you can focus on creating and monitoring your ETL work\. The console performs administrative and job development operations on your behalf\. You supply credentials and other properties to AWS Glue to access your data sources and write to your data warehouse\.

AWS Glue takes care of provisioning and managing the resources that are required to run your workload\. You don't need to create the infrastructure for an ETL tool because AWS Glue does it for you\. When resources are required, to reduce startup time, AWS Glue uses an instance from its warm pool of instances to run your workload\.

With AWS Glue, you create jobs using table definitions in your Data Catalog\. Jobs consist of scripts that contain the programming logic that performs the transformation\. You use triggers to initiate jobs either on a schedule or as a result of a specified event\. You determine where your target data resides and which source data populates your target\. With your input, AWS Glue generates the code that's required to transform your data from source to target\. You can also provide scripts in the AWS Glue console or API to process your data\.


+ [AWS Glue Concepts](components-key-concepts.md)
+ [AWS Glue Components](components-overview.md)
+ [Converting Semi\-Structured Schemas to Relational Schemas](schema-relationalize.md)