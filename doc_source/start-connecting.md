# Setting Up Your Environment to Access Data Stores<a name="start-connecting"></a>

To run your extract, transform, and load \(ETL\) jobs, AWS Glue must be able to access your data stores\. If a job doesn't need to run in your virtual private cloud \(VPC\) subnet—for example, transforming data from Amazon S3 to Amazon S3—no additional configuration is needed\.

If a job needs to run in your VPC subnet, AWS Glue  sets up [elastic network interfaces](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ElasticNetworkInterfaces.html) that enable your jobs to connect securely to other resources within your VPC\. Each elastic network interface is assigned a private IP address from the IP address range within the subnet you specified\. No public IP addresses are assigned\. The specified security groups are applied on the elastic network interface\.

All JDBC data stores that are accessed by the job must be available from the VPC subnet\. To access Amazon S3 from within your VPC, a VPC endpoint is required\. If your job needs to access both VPC resources and the public internet,  the VPC needs to have a Network Address Translation \(NAT\) gateway inside the VPC\.

For JDBC data stores, you create a connection in AWS Glue with the necessary properties to connect to your data stores\. For more information about the connection, see Adding a Connection to Your Data Store\.


+ [Amazon VPC Endpoints for Amazon S3](vpc-endpoints-s3.md)
+ [Setting Up a VPC to Connect to JDBC Data Stores](setup-vpc-for-glue-access.md)
+ [Setting Up DNS in Your VPC](set-up-vpc-dns.md)