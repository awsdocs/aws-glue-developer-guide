# Troubleshooting Errors in AWS Glue<a name="glue-troubleshooting-errors"></a>

If you encounter errors in AWS Glue, use the following solutions to help you find the source of the problems and fix them\.

**Note**  
 The AWS Glue GitHub repository contains additional troubleshooting guidance in [ AWS Glue Frequently Asked Questions](https://github.com/awslabs/aws-glue-samples/blob/master/FAQ_and_How_to.md)\. 

## Error: Resource Unavailable<a name="error-resource-unavailable"></a>

If AWS Glue returns a resource unavailable message, you can view error messages or logs to help you learn more about the issue\. The following tasks describe general methods for troubleshooting\.

+ For any connections and development endpoints that you use, check that your cluster has not run out of elastic network interfaces\.

## Error: Could Not Find S3 Endpoint or NAT Gateway for subnetId in VPC<a name="error-s3-subnet-vpc-NAT-configuration"></a>

Check the subnet ID and VPC ID in the message to help you diagnose the issue\.

+ Check that you have an Amazon S3 VPC endpoint set up, which is required with AWS Glue\. In addition, check your NAT gateway if that's part of your configuration\. For more information, see [Amazon VPC Endpoints for Amazon S3](vpc-endpoints-s3.md)\.

## Error: Inbound Rule in Security Group Required<a name="error-inbound-self-reference-rule"></a>

At least one security group must open all ingress ports\. To limit traffic, the source security group in your inbound rule can be restricted to the same security group\.

+ For any connections that you use, check your security group for an inbound rule that is self\-referencing\. For more information, see [Setting Up Your Environment to Access Data Stores](start-connecting.md)\.

+ When you are using a development endpoint, check your security group for an inbound rule that is self\-referencing\. For more information, see [Setting Up Your Environment to Access Data Stores](start-connecting.md)\.

## Error: Outbound Rule in Security Group Required<a name="error-outbound-self-reference-rule"></a>

At least one security group must open all egress ports\. To limit traffic, the source security group in your outbound rule can be restricted to the same security group\.

+ For any connections that you use, check your security group for an outbound rule that is self\-referencing\. For more information, see [Setting Up Your Environment to Access Data Stores](start-connecting.md)\.

+ When you are using a development endpoint, check your security group for an outbound rule that is self\-referencing\. For more information, see [Setting Up Your Environment to Access Data Stores](start-connecting.md)\.

## Error: Custom DNS Resolution Failures<a name="error-custom-dns"></a>

When using a custom DNS for internet name resolution, both forward DNS lookup and reverse DNS lookup must be implemented\. Otherwise, you might receive errors similar to: *Reverse dns resolution of ip failure* or *Dns resolution of dns failed*\. If AWS Glue returns a message, you can view error messages or logs to help you learn more about the issue\. The following tasks describe general methods for troubleshooting\.

+ A custom DNS configuration without reverse lookup can cause AWS Glue to fail\. Check your DNS configuration\. If you are using Route 53 or Microsoft Active Directory, make sure that there are forward and reverse lookups\. For more information, see [Setting Up DNS in Your VPC](set-up-vpc-dns.md)\.

## Error: Job run failed because the role passed should be given assume role permissions for the AWS Glue Service<a name="error-assume-role-user-policy"></a>

The user who defines a job must have permission for `iam:PassRole` for AWS Glue\.

+ When a user creates an AWS Glue job, confirm that the user's role contains a policy that contains `iam:PassRole` for AWS Glue\. For more information, see [Step 3: Attach a Policy to IAM Users That Access AWS Glue](attach-policy-iam-user.md)\.

## Error: DescribeVpcEndpoints Action Is Unauthorized\. Unable to Validate VPC ID vpc\-id<a name="error-DescribeVpcEndpoints-permission"></a>

+ Check the policy passed to AWS Glue for the `ec2:DescribeVpcEndpoints` permission\.

## Error: DescribeRouteTables Action Is Unauthorized\. Unable to Validate Subnet Id: subnet\-id in VPC id: vpc\-id<a name="error-DescribeRouteTables-permission"></a>

+ Check the policy passed to AWS Glue for the `ec2:DescribeRouteTables` permission\.

## Error: Failed to Call ec2:DescribeSubnets<a name="error-DescribeSubnets-permission"></a>

+ Check the policy passed to AWS Glue for the `ec2:DescribeSubnets` permission\.

## Error: Failed to Call ec2:DescribeSecurityGroups<a name="error-DescribeSecurityGroups-permission"></a>

+ Check the policy passed to AWS Glue for the `ec2:DescribeSecurityGroups` permission\.

## Error: Could Not Find Subnet for AZ<a name="error-az-not-available"></a>

+ The Availability Zone might not be available to AWS Glue\. Create and use a new subnet in a different Availability Zone from the one specified in the message\.

## Error: Job Run Exception for Connection List with Multiple Subnet or AZ<a name="error-connection-multiple-az-in-job"></a>

When running a job, validation fails with the exception: `CONNECTION_LIST_CONNECTION_WITH_MULTIPLE_SUBNET_ID` and `CONNECTION_LIST_CONNECTION_WITH_MULTIPLE_AZ`\.

+ If your job has multiple connections, they can't be in different Availability Zones or subnets\. Ensure that all connections in a job are in the same Availability Zone, or edit the job to remove connections so that only connections that are in the same Availability Zone are required\.

## Error: Job Run Exception When Writing to a JDBC Target<a name="error-job-run-jdbc-target"></a>

When you are running a job that writes to a JDBC target, the job might encounter errors in the following scenarios:

+ If your job writes to a Microsoft SQL Server table, and the table has columns defined as type `Boolean`, then the table must be predefined in the SQL Server database\. When you define the job on the AWS Glue console using a SQL Server target with the option **Create tables in your data target**, don't map any source columns to a target column with data type `Boolean`\. You might encounter an error when the job runs\.

  You can avoid the error by doing the following:

  + Choose an existing table with the **Boolean** column\.

  + Edit the `ApplyMapping` transform and map the **Boolean** column in the source to a number or string in the target\.

  + Edit the `ApplyMapping` transform to remove the **Boolean** column from the source\.

+ If your job writes to an Oracle table, you might need to adjust the length of names of Oracle objects\. In some versions of Oracle, the maximum identifier length is limited to 30 bytes or 128 bytes\. This limit affects the table names and column names of Oracle target data stores\.

  You can avoid the error by doing the following:

  + Name Oracle target tables within the limit for your version\.

  + The default column names are generated from the field names in the data\. To handle the case when the column names are longer than the limit, use `ApplyMapping` or `RenameField` transforms to change the name of the column to be within the limit\.

## Error: Amazon S3 Timeout<a name="error-s3-timeout"></a>

If AWS Glue returns a connect timed out error, it might be because it is trying to access an Amazon S3 bucket in another AWS Region\. 

+ An Amazon S3 VPC endpoint can only route traffic to buckets within an AWS Region\. If you need to connect to buckets in other Regions, a possible workaround is to use a NAT gateway\. For more information, see [NAT Gateways](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway.html)\.

## Error: No Private DNS for Network Interface Found<a name="error-no-private-DNS"></a>

If a job fails or a development endpoint fails to provision, it might be because of a problem in the network setup\.

+ If you are using the Amazon provided DNS, the value of `enableDnsHostnames` must be set to true\. For more information, see [DNS](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-dns.html)\. 

## Error: Development Endpoint Provisioning Failed<a name="error-development-endpoint-failed"></a>

If AWS Glue fails to successfully provision a development endpoint, it might be because of a problem in the network setup\.

+ When you define a development endpoint, the VPC, subnet, and security groups are validated to confirm that they meet certain requirements\.

+ If you provided the optional SSH public key, check that it is a valid SSH public key\.

+ Check in the VPC console that your VPC uses a valid **DHCP option set**\. For more information, see [DHCP option sets](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_DHCP_Options.html)\. 

+ If the cluster remains in the PROVISIONING state, contact AWS Support\.

## Error: Notebook Server CREATE\_FAILED<a name="error-notebook-server-ec2-instance-profile"></a>

If AWS Glue fails to create the notebook server for a development endpoint, it might be because of one of the following problems: 

+ AWS Glue passes an IAM role to Amazon EC2 when it is setting up the notebook server\. The IAM role must have a trust relationship to Amazon EC2\.

+ The IAM role must have an instance profile of the same name\. When you create the role with the IAM console, the instance profile with the same name is automatically created\. Check for an error in the log regarding an invalid instance profile name `iamInstanceProfile.name`\. For more information, see [Using Instance Profiles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html)\. 

+ Check that your role has permission to access `aws-glue*` buckets in the policy that you pass to create the notebook server\. 

## Error: Notebook Usage Errors<a name="error-notebook-usage-errors"></a>

When using an Apache Zeppelin notebook, you might encounter errors due to your setup or environment\. 

+ You provide an IAM role with an attached policy when you created the notebook server\. If the policy does not include all the required permissions, you might get an error such as `assumed-role/name-of-role/i-0bf0fa9d038087062 is not authorized to perform some-action AccessDeniedException`\. Check the policy that is passed to your notebook server in the IAM console\. 

+ If the Zeppelin notebook does not render correctly in your web browser, check the Zeppelin requirements for browser support\. For example, there might be specific versions and setup required for the Safari browser\. You might need to update your browser or use a different browser\. 

## Error: Running Crawler Failed<a name="error-running-crawler-failed"></a>

If AWS Glue fails to successfully run a crawler to catalog your data, it might be because of one of the following reasons\. First check if an error is listed in the AWS Glue console crawlers list\. Check if there is an exclamation  icon next to the crawler name and hover over the icon to see any associated messages\. 

+ Check the logs for the crawler run in CloudWatch Logs under `/aws-glue/crawlers`\. The URL link to the crawler logs on the AWS Glue console contains both the crawler name and the crawler ID\. Also, the first record of the crawler log in CloudWatch Logs contains the crawler ID for that run\.

## Error: Upgrading Athena Data Catalog<a name="error-running-athena-upgrade"></a>

If you encounter errors while upgrading your Athena Data Catalog to the AWS Glue Data Catalog, see the Amazon Athena User Guide topic [Upgrading to the AWS Glue Data Catalog Step\-by\-Step](http://docs.aws.amazon.com/athena/latest/ug/glue-upgrade.html)\. 