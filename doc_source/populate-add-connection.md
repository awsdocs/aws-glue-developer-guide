# Adding a Connection to Your Data Store<a name="populate-add-connection"></a>

Connections are used by crawlers and jobs in AWS Glue to access certain types of data stores\. For information about adding a connection using the AWS Glue console, see [Working with Connections on the AWS Glue Console](console-connections.md)\.

## When Is a Connection Used?<a name="connection-using"></a>

If your data store requires one, the connection is used when you crawl a data store to catalog its metadata in the AWS Glue Data Catalog\. The connection is also used by any job that uses the data store as a source or target\.

## Defining a Connection in the AWS Glue Data Catalog<a name="connection-defining"></a>

Some types of data stores require additional connection information to access your data\. This information might include an additional user name and password \(different from your AWS credentials\),  or other information that is required to connect to the data store\.

After AWS Glue connects to a JDBC data store, it must have permission from the data store to perform operations\. The username you provide with the connection must have the required permissions or privileges\. For example, a crawler requires SELECT privileges to retrieve metadata from a JDBC data store\. Likewise, a job that writes to a JDBC target requires the necessary privileges to INSERT, UPDATE, and DELETE data into an existing table\.

AWS Glue can connect to the following data stores by using the JDBC protocol:
+ Amazon Redshift
+ Amazon Relational Database Service
  + Amazon Aurora
  + MariaDB
  + Microsoft SQL Server
  + MySQL
  + Oracle
  + PostgreSQL
+ Publicly accessible databases
  + Amazon Aurora
  + MariaDB
  + Microsoft SQL Server
  + MySQL
  + Oracle
  + PostgreSQL

A connection is not typically required for Amazon S3\. However, to access Amazon S3 from within your virtual private cloud \(VPC\), an Amazon S3 VPC endpoint is required\. For more information, see [Amazon VPC Endpoints for Amazon S3](vpc-endpoints-s3.md)\. 

In your connection information, you also must consider whether data is accessed through a VPC and then set up network parameters accordingly\.  AWS Glue requires a private IP for JDBC endpoints\. Connections to databases can be over a VPN and DirectConnect as they provide private IP access to on\-premises databases\.

For information about how to connect to on\-premises databases, see the blog post [How to access and analyze on\-premises data stores using AWS Glue](https://aws.amazon.com/blogs/big-data/how-to-access-and-analyze-on-premises-data-stores-using-aws-glue/)\. 

## Connecting to a JDBC Data Store in a VPC<a name="connection-JDBC-VPC"></a>

Typically, you create resources inside Amazon Virtual Private Cloud \(Amazon VPC\) so that they cannot be accessed over the public internet\. By default, resources in a VPC can't be accessed from AWS Glue\. To enable AWS Glue to access resources inside your VPC, you must provide additional VPC\-specific configuration information that includes VPC subnet IDs and security group IDs\. AWS Glue uses this information to set up [elastic network interfaces](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_ElasticNetworkInterfaces.html) that enable your function to connect securely to other resources in your private VPC\.

### Accessing VPC Data Using Elastic Network Interfaces<a name="connection-JDBC-VPC-ENI"></a>

When AWS Glue connects to a JDBC data store in a VPC, AWS Glue creates an elastic network interface \(with the prefix `Glue_`\) in your account to access your VPC data\. You can't delete this network interface as long as it's attached to AWS Glue\. As part of creating the elastic network interface, AWS Glue associates one or more security groups to it\. To enable AWS Glue to create the network interface, security groups that are associated with the resource must allow inbound access with a source rule\. This rule contains a security group that is associated with the resource\. This gives the elastic network interface access to your data store with the same security group\.

To allow AWS Glue to communicate with its components, specify a security group with a self\-referencing inbound rule for all TCP ports\. By creating a self\-referencing rule, you can restrict the source to the same security group in the VPC and not open it to all networks\. The default security group for your VPC might already have a self\-referencing inbound rule for `ALL Traffic`\.

You can create rules in the Amazon VPC console\. To update rule settings via the AWS Management Console, navigate to the VPC console \([https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\), and select the appropriate security group\. Specify the inbound rule for `ALL TCP` to have as its source the same security group name\. For more information about security group rules, see [Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)\.

Each elastic network interface is assigned a private IP address from the IP address range in the subnets that you specify\. The network interface is not assigned any public IP addresses\. AWS Glue requires internet access \(for example, to access AWS services that don't have VPC endpoints\)\. You can configure a network address translation \(NAT\) instance inside your VPC, or you can use the Amazon VPC NAT gateway\. For more information, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) in the *Amazon VPC User Guide*\. You can't directly use an internet gateway attached to your VPC as a route in your subnet route table because that requires the network interface to have public IP addresses\.

The VPC network attributes `enableDnsHostnames` and `enableDnsSupport` must be set to true\. For more information, see [Using DNS with your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html)\. 

**Important**  
Don't put your data store in a public subnet or in a private subnet that doesn't have internet access\. Instead, attach it only to private subnets that have internet access through a NAT instance or an Amazon VPC NAT gateway\.

### Elastic Network Interface Properties<a name="connection-JDBC-VPC-ENI-properties"></a>

To create the elastic network interface, you must supply the following properties:

**VPC**  
The name of the VPC that contains your data store\.

**Subnet**  
The subnet in the VPC that contains your data store\.  

**Security groups**  
The security groups that are associated with your data store\. AWS Glue associates these security groups with the elastic network interface that is attached to your VPC subnet\. To allow AWS Glue components to communicate and also prevent access from other networks, at least one chosen security group must specify a self\-referencing inbound rule for all TCP ports\.

For information about managing a VPC with Amazon Redshift, see [Managing Clusters in an Amazon Virtual Private Cloud \(VPC\)](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-vpc.html)\.

For information about managing a VPC with Amazon RDS, see [Working with an Amazon RDS DB Instance in a VPC](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html)\.