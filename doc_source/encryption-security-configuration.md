# Encrypting Data Written by Crawlers, Jobs, and Development Endpoints<a name="encryption-security-configuration"></a>

A *security configuration* is a set of security properties that can be used by AWS Glue\. You can use a security configuration to encrypt data at rest\. The following scenarios show some of the ways that you can use a security configuration\. 
+ Attach a security configuration to an AWS Glue crawler to write encrypted Amazon CloudWatch Logs\.
+ Attach a security configuration to an extract, transform, and load \(ETL\) job to write encrypted Amazon Simple Storage Service \(Amazon S3\) targets and encrypted CloudWatch Logs\.
+ Attach a security configuration to an ETL job to write its jobs bookmarks as encrypted Amazon S3 data\.
+ Attach a security configuration to a development endpoint to write encrypted Amazon S3 targets\.


|  | 
| --- |
|   Currently, a security configuration overrides any server\-side encryption \(SSE\-S3\) setting that is passed as an ETL job parameter\. Thus, if both a security configuration and an SSE\-S3 parameter are associated with a job, the SSE\-S3 parameter is ignored\.   | 

For more information about security configurations, see [Working with Security Configurations on the AWS Glue Console](console-security-configurations.md)\.

**Topics**
+ [Setting Up AWS Glue to Use Security Configurations](#encryption-setup-Glue)
+ [Creating a Route to AWS KMS for VPC Jobs and Crawlers](#encryption-kms-vpc-endpoint)
+ [Working with Security Configurations on the AWS Glue Console](console-security-configurations.md)

## Setting Up AWS Glue to Use Security Configurations<a name="encryption-setup-Glue"></a>

Follow these steps to set up your AWS Glue environment to use security configurations\.

1. Create or update your AWS Key Management Service \(AWS KMS\) keys to allow AWS KMS permissions to the IAM roles that are passed to AWS Glue crawlers and jobs to encrypt CloudWatch Logs\. For more information, see [Encrypt Log Data in CloudWatch Logs Using AWS KMS](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/encrypt-log-data-kms.html) in the *Amazon CloudWatch Logs User Guide*\. 

   In the following example, *"role1"*, *"role2"*, and *"role3"* are IAM roles that are passed to crawlers and jobs:

   ```
   {
          "Effect": "Allow",
          "Principal": { "Service": "logs.region.amazonaws.com",
          "AWS": [
                   "role1",
                   "role2",
                   "role3"
                ] },
                       "Action": [
                              "kms:Encrypt*",
                              "kms:Decrypt*",
                              "kms:ReEncrypt*",
                              "kms:GenerateDataKey*",
                              "kms:Describe*"
                       ],
                       "Resource": "*"
   }
   ```

   The `Service` statement, shown as `"Service": "logs.region.amazonaws.com"`, is required if you use the key to encrypt CloudWatch Logs\.

1. Ensure that the AWS KMS key is `ENABLED` before it is used\.

1. Ensure that the AWS Glue job includes the following code for the security setting to take effect:

   ```
               job = Job(glueContext) 
               job.init(args['JOB_NAME'], args)
   ```

## Creating a Route to AWS KMS for VPC Jobs and Crawlers<a name="encryption-kms-vpc-endpoint"></a>

You can connect directly to AWS KMS through a private endpoint in your virtual private cloud \(VPC\) instead of connecting over the internet\. When you use a VPC endpoint, communication between your VPC and AWS KMS is conducted entirely within the AWS network\.

You can create an AWS KMS VPC endpoint within a VPC\. Without this step, your jobs or crawlers might fail with a `kms timeout` on jobs or an `internal service exception` on crawlers\. For detailed instructions, see [Connecting to AWS KMS Through a VPC Endpoint](https://docs.aws.amazon.com/kms/latest/developerguide/kms-vpc-endpoint.html) in the *AWS Key Management Service Developer Guide*\. 

As you follow these instructions, on the [VPC console](https://console.aws.amazon.com//vpc), you must do the following:
+ Select the **Enable Private DNS name** check box\.
+ Choose the **Security group** \(with self\-referencing rule\) that you use for your job or crawler that accesses Java Database Connectivity \(JDBC\)\. For more information about AWS Glue connections, see [Adding a Connection to Your Data Store](populate-add-connection.md)\.

When you add a security configuration to a crawler or job that accesses JDBC data stores, AWS Glue must have a route to the AWS KMS endpoint\. You can provide the route with a network address translation \(NAT\) gateway or with an AWS KMS VPC endpoint\. To create a NAT gateway, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) in the *Amazon VPC User Guide*\.