# Crawling an Amazon S3 Data Store using a VPC Endpoint<a name="connection-S3-VPC"></a>

For security, auditing, or control purposes you may want your Amazon S3 data store to only be accessed through an Amazon Virtual Private Cloud environment \(Amazon VPC\)\. This topic describes how to create and test a connection to the Amazon S3 data store in a VPC endpoint using the `Network` connection type\.

Perform the following tasks to run a crawler on the data store:
+ [Prerequisites](#connection-S3-VPC-prerequisites)
+ [Creating the Connection to Amazon S3](#connection-S3-VPC-create-connection)
+ [Testing the Connection to Amazon S3](#connection-S3-VPC-test-connection)
+ [Creating a Crawler](#connection-S3-VPC-create-crawler)
+ [Running a Crawler](#connection-S3-VPC-run-crawler)

## Prerequisites<a name="connection-S3-VPC-prerequisites"></a>

Check that you have met these prerequisites for setting up your Amazon S3 data store to be accessed through an Amazon Virtual Private Cloud environment \(Amazon VPC\)
+ A configured VPC\. For example: vpc\-01685961063b0d84b\. For more information, see [Getting started with Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-getting-started.html) in the *Amazon VPC User Guide*\.
+ An Amazon S3 endpoint attached to the VPC\. For example: vpc\-01685961063b0d84b\. For more information, see [Endpoints for Amazon S3](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-s3.html) in the *Amazon VPC User Guide*\.  
![\[Example of an Amazon S3 endpoint attached to a VPC.\]](http://docs.aws.amazon.com/glue/latest/dg/images/network_s3_vpc_s3_endpoint_attached.png)
+ A route entry pointing to the VPC endpoint\. For example vpce\-0ec5da4d265227786 in the route table used by the VPC endpoint\(vpce\-0ec5da4d265227786\)\.  
![\[Example of a route entry pointing to the VPC endpoint.\]](http://docs.aws.amazon.com/glue/latest/dg/images/network_s3_vpc_route_entry.png)
+ A network ACL attached to the VPC allows the traffic\.
+ A security group attached to the VPC allows the traffic\.

## Creating the Connection to Amazon S3<a name="connection-S3-VPC-create-connection"></a>

Typically, you create resources inside Amazon Virtual Private Cloud \(Amazon VPC\) so that they cannot be accessed over the public internet\. By default, AWS Glue can't access resources inside a VPC\. To enable AWS Glue to access resources inside your VPC, you must provide additional VPC\-specific configuration information that includes VPC subnet IDs and security group IDs\. To create a `Network` connection you need to specify the following information:
+ A VPC ID
+ A subnet within the VPC
+ A security group

To set up a `Network` connection:

1. Choose **Add connection** in the navigation pane of the AWS Glue console\.

1. Enter the connection name, choose **Network** as the connection type\. Choose **Next**\.  
![\[Selecting the connection type.\]](http://docs.aws.amazon.com/glue/latest/dg/images/network_s3_vpc_add_network_1.png)

1. Configure the VPC, Subnet and Security groups information\.
   + VPC: choose the VPC name that contains your data store\.
   + Subnet: choose the subnet within your VPC\.
   + Security groups: choose one or more security groups that allow access to the data store in your VPC\.  
![\[Selecting the connection type.\]](http://docs.aws.amazon.com/glue/latest/dg/images/network_s3_vpc_add_network_2.png)

1. Choose **Next**\.

1. Verify the connection information and choose **Finish**\.  
![\[Selecting the connection type.\]](http://docs.aws.amazon.com/glue/latest/dg/images/network_s3_vpc_add_network_3.png)

## Testing the Connection to Amazon S3<a name="connection-S3-VPC-test-connection"></a>

Once you have created your `Network` connection, you can test the connectivity to your Amazon S3 data store in a VPC endpoint\.

The following errors may occur when testing a connection:
+ INTERNET CONNECTION ERROR: indicates an Internet connection issue
+ INVALID BUCKET ERROR: indicates a problem with the Amazon S3 bucket
+ S3 CONNECTION ERROR: indicates a failure to connect to Amazon S3
+ INVALID CONNECTION TYPE: indicates the Connection type does not have the expected value, `NETWORK`
+ INVALID CONNECTION TEST TYPE: indicates a problem with the type of network connection test
+ INVALID TARGET: indicates that the Amazon S3 bucket has not been specified properly

To test a `Network` connection:

1. Select the **Network** connection in the AWS Glue console\.

1. Choose **Test connection**\.

1. Choose the IAM role that you created in the previous step and specify an Amazon S3 Bucket\.

1. Choose **Test connection** to start the test\. It might take few moments to show the result\. 

![\[Testing the connection.\]](http://docs.aws.amazon.com/glue/latest/dg/images/network_s3_vpc_test_network.png)

 If you receive an error, check the following:
+ The correct privileges are provided to the role selected\.
+ The correct Amazon S3 bucket is provided\.
+ The security groups and Network ACL allow the required incoming and outgoing traffic\.
+ The VPC you specified is connected to an Amazon S3 VPC endpoint\.

Once you have successfully tested the connection, you can create a crawler\.

## Creating a Crawler<a name="connection-S3-VPC-create-crawler"></a>

You can now create a crawler that specifies the `Network` connection you've created\. For more details on creating a crawler, see [Working with Crawlers on the AWS Glue Console](console-crawlers.md)\.

1. Start by choosing **Crawlers** in the navigation pane on the AWS Glue console\. 

1. Choose **Add crawler**\.

1. Specify the crawler name and choose **Next**\.

1. When asked for the data source, choose **S3**, and specify the Amazon S3 bucket prefix and the connection you created earlier\.  
![\[Testing the connection.\]](http://docs.aws.amazon.com/glue/latest/dg/images/network_s3_vpc_add_crawler_1.png)

1. If you need to, add another data store on the same network connection\.

1. Choose IAM role\. The IAM role must allow access to the AWS Glue service and the Amazon S3 bucket\. For more information, see [Working with Crawlers on the AWS Glue Console](console-crawlers.md)\.  
![\[Testing the connection.\]](http://docs.aws.amazon.com/glue/latest/dg/images/network_s3_vpc_add_crawler_2.png)

1. Define the schedule for the crawler\.

1. Choose an existing database in the Data Catalog, or create a new database entry\.  
![\[Testing the connection.\]](http://docs.aws.amazon.com/glue/latest/dg/images/network_s3_vpc_add_crawler_3.png)

1. Finish the remaining setup\.

## Running a Crawler<a name="connection-S3-VPC-run-crawler"></a>

Run your crawler\.

![\[Running your crawler on demand.\]](http://docs.aws.amazon.com/glue/latest/dg/images/network_s3_vpc_s3_endpoint_run_crawler.png)

## Troubleshooting<a name="connection-S3-VPC-troubleshooting"></a>

For troubleshooting related to Amazon S3 buckets using a VPC gateway, see [Why can’t I connect to an S3 bucket using a gateway VPC endpoint?](https://aws.amazon.com/premiumsupport/knowledge-center/connect-s3-vpc-endpoint/)