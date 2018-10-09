# Amazon VPC Endpoints for Amazon S3<a name="vpc-endpoints-s3"></a>

For security reasons, many AWS customers run their applications within an Amazon Virtual Private Cloud environment \(Amazon VPC\)\. With Amazon VPC, you can launch Amazon EC2 instances into a virtual private cloud, which is logically isolated from other networks—including the public internet\. With an Amazon VPC, you have control over its IP address range, subnets, routing tables, network gateways, and security settings\.

**Note**  
If you created your AWS account after 2013\-12\-04, you already have a default VPC in each AWS Region\. You can immediately start using your default VPC without any additional configuration\.  
For more information, see [Your Default VPC and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html) in the Amazon VPC User Guide\.

Many customers have legitimate privacy and security concerns about sending and receiving data across the public internet\. Customers can address these concerns by using a virtual private network \(VPN\) to route all Amazon S3 network traffic through their own corporate network infrastructure\. However, this approach can introduce bandwidth and availability challenges\.

VPC endpoints for Amazon S3 can alleviate these challenges\. A VPC endpoint for Amazon S3 enables AWS Glue to use private IP addresses to access Amazon S3 with no exposure to the public internet\. AWS Glue does not require public IP addresses, and you don't need an internet gateway, a NAT device, or a virtual private gateway in your VPC\. You use endpoint policies to control access to Amazon S3\. Traffic between your VPC and the AWS service does not leave the Amazon network\.

When you create a VPC endpoint for Amazon S3, any requests to an Amazon S3 endpoint within the Region \(for example, *s3\.us\-west\-2\.amazonaws\.com*\) are routed to a private Amazon S3 endpoint within the Amazon network\. You don't need to modify your applications running on EC2 instances in your VPC—the endpoint name remains the same, but the route to Amazon S3 stays entirely within the Amazon network, and does not access the public internet\.

For more information about VPC endpoints, see [VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) in the Amazon VPC User Guide\.

The following diagram shows how AWS Glue can use a VPC endpoint to access Amazon S3\.

![\[Network traffic flow showing VPC connection to Amazon S3.\]](http://docs.aws.amazon.com/glue/latest/dg/images/PopulateCatalog-vpc-endpoint.png)

**To set up access for Amazon S3**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the left navigation pane, choose **Endpoints**\.

1. Choose **Create Endpoint**, and follow the steps to create an Amazon S3 endpoint in your VPC\.