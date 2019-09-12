# Using AWS Glue with VPC Endpoints<a name="vpc-endpoint"></a>

If you use Amazon Virtual Private Cloud \(Amazon VPC\) to host your AWS resources, you can establish a private connection between your VPC and AWS Glue\. You use this connection to enable AWS Glue to communicate with the resources in your VPC without going through the public internet\.

Amazon VPC is an AWS service that you can use to launch AWS resources in a virtual network that you define\. With a VPC, you have control over your network settings, such the IP address range, subnets, route tables, and network gateways\. To connect your VPC to AWS Glue, you define an interface VPC endpoint for AWS Glue\. When you use a VPC interface endpoint, communication between your VPC and AWS Glue is conducted entirely and securely within the AWS network\.

You can use AWS Glue with VPC endpoints in all AWS Regions that support both AWS Glue and Amazon VPC endpoints\.

For more information, see these topics in the *Amazon VPC User Guide*:
+ [What Is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
+ [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint)