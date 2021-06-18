# Setting Up Your Environment to Access Data Stores<a name="start-connecting"></a>

To run your extract, transform, and load \(ETL\) jobs, AWS Glue must be able to access your data stores\. If a job doesn't need to run in your virtual private cloud \(VPC\) subnet—for example, transforming data from Amazon S3 to Amazon S3—no additional configuration is needed\.

If a job needs to run in your VPC subnet—for example, transforming data from a JDBC data store in a private subnet—AWS Glue  sets up [elastic network interfaces](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_ElasticNetworkInterfaces.html) that enable your jobs to connect securely to other resources within your VPC\. Each elastic network interface is assigned a private IP address from the IP address range within the subnet you specified\. No public IP addresses are assigned\. Security groups specified in the AWS Glue connection are applied on each of the elastic network interfaces\. For more information, see [Setting Up a VPC to Connect to JDBC Data Stores](setup-vpc-for-glue-access.md)\.  

All JDBC data stores that are accessed by the job must be available from the VPC subnet\. To access Amazon S3 from within your VPC, a [VPC endpoint](vpc-endpoints-s3.md) is required\. If your job needs to access both VPC resources and the public internet,  the VPC needs to have a Network Address Translation \(NAT\) gateway inside the VPC\.

 A job or development endpoint can only access one VPC \(and subnet\) at a time\. If you need to access data stores in different VPCs, you have the following options: 
+ Use VPC peering to access the data stores\. For more about VPC peering, see [VPC Peering Basics](https://docs.aws.amazon.com/vpc/latest/peering/vpc-peering-basics.html) 
+ Use an Amazon S3 bucket as an intermediary storage location\. Split the work into two jobs, with the Amazon S3 output of job 1 as the input to job 2\.

For JDBC data stores, you create a connection in AWS Glue with the necessary properties to connect to your data stores\. For more information about the connection, see [Adding a Connection to Your Data Store](populate-add-connection.md)\.

**Note**  
Make sure you set up your DNS environment for AWS Glue\. For more information, see [Setting Up DNS in Your VPC](set-up-vpc-dns.md)\. 

**Topics**
+ [Amazon VPC Endpoints for Amazon S3](vpc-endpoints-s3.md)
+ [Setting Up a VPC to Connect to JDBC Data Stores](setup-vpc-for-glue-access.md)