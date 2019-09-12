# AWS Glue: How It Works<a name="how-it-works"></a>

AWS Glue uses other AWS services to orchestrate your ETL \(extract, transform, and load\) jobs to build a data warehouse\. AWS Glue calls API operations to transform your data, create runtime logs, store your job logic, and create notifications to help you monitor your job runs\. The AWS Glue console connects these services into a managed application, so you can focus on creating and monitoring your ETL work\. The console performs administrative and job development operations on your behalf\. You supply credentials and other properties to AWS Glue to access your data sources and write to your data warehouse\.

AWS Glue takes care of provisioning and managing the resources that are required to run your workload\. You don't need to create the infrastructure for an ETL tool because AWS Glue does it for you\. When resources are required, to reduce startup time, AWS Glue uses an instance from its warm pool of instances to run your workload\.

With AWS Glue, you create jobs using table definitions in your Data Catalog\. Jobs consist of scripts that contain the programming logic that performs the transformation\. You use triggers to initiate jobs either on a schedule or as a result of a specified event\. You determine where your target data resides and which source data populates your target\. With your input, AWS Glue generates the code that's required to transform your data from source to target\. You can also provide scripts in the AWS Glue console or API to process your data\.

AWS Glue is available in several AWS Regions\. For more information, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the Amazon Web Services General Reference\.

**Topics**
+ [Serverless ETL Jobs Run in Isolation](#how-it-works-isolation)
+ [AWS Glue Concepts](components-key-concepts.md)
+ [AWS Glue Components](components-overview.md)
+ [Converting Semi\-Structured Schemas to Relational Schemas](schema-relationalize.md)

## Serverless ETL Jobs Run in Isolation<a name="how-it-works-isolation"></a>

AWS Glue runs your ETL jobs in an Apache Spark serverless environment\. AWS Glue runs these jobs on virtual resources that it provisions and manages in its own service account\. 

AWS Glue is designed to do the following:
+ Segregate customer data\.
+ Protect customer data in transit and at rest\.
+ Access customer data only as needed in response to customer requests, using temporary, scoped\-down credentials, or with a customer's consent to IAM roles in their account\.

During provisioning of an ETL job, you provide input data sources and output data targets in your virtual private cloud \(VPC\)\. In addition, you provide the IAM role, VPC ID, subnet ID, and security group that are needed to access data sources and targets\. For each tuple \(customer account ID, IAM role, subnet ID, and security group\), AWS Glue creates a new Spark environment that is isolated at the network and management level from all other Spark environments inside the AWS Glue service account\.

AWS Glue creates elastic network interfaces in your subnet using private IP addresses\. Spark jobs use these elastic network interfaces to access your data sources and data targets\. Traffic in, out, and within the Spark environment is governed by your VPC and networking policies with one exception: Calls made to AWS Glue libraries can proxy traffic to AWS Glue API operations through the AWS Glue VPC\. All AWS Glue API calls are logged; thus, data owners can audit API access by enabling [AWS CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/), which delivers audit logs to your account\.

AWS Glue managed Spark environments that run your ETL jobs are protected with the same security practices followed by other AWS services\. Those practices are listed in the **AWS Access** section of the [Introduction to AWS Security Processes](https://d1.awsstatic.com/whitepapers/Security/Intro_Security_Practices.pdf) whitepaper\.