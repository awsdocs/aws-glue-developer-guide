# Setting Up Your Environment for Development Endpoints<a name="start-development-endpoint"></a>

To run your extract, transform, and load \(ETL\) scripts with AWS Glue, you sometimes develop and test your scripts using a development endpoint\. When you set up a development endpoint, you specify a virtual private cloud \(VPC\), subnet, and security groups\. 

**Note**  
Make sure you set up your DNS environment for AWS Glue\. For more information, see [Setting Up DNS in Your VPC](set-up-vpc-dns.md)\. 

## Setting Up Your Network for a Development Endpoint<a name="setup-vpc-for-development-endpoint"></a>

 To enable AWS Glue to access required resources, add a row in your subnet route table to associate a prefix list for Amazon S3 to the VPC endpoint\. A prefix list ID is required for creating an outbound security group rule that allows traffic from a VPC to access an AWS service through a VPC endpoint\.  To ease connecting to a notebook server that is associated with this development endpoint, from your local machine, add a row to the route table to add an internet gateway ID\. For more information, see [VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html)\. Update the subnet routes table to be similar to the following table:  


****  

| Destination | Target | 
| --- | --- | 
| 10\.0\.0\.0/16 | local | 
| pl\-id for Amazon S3 | vpce\-id | 
| 0\.0\.0\.0/0 | igw\-xxxx | 

 To enable AWS Glue to communicate between its components, specify a security group with a self\-referencing inbound rule for all TCP ports\. By creating a self\-referencing rule, you can restrict the source to the same security group in the VPC, and it's not open to all networks\. The default security group for your VPC might already have a self\-referencing inbound rule for ALL Traffic\. 

**To set up a security group**

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the left navigation pane, choose **Security Groups**\.

1. Either choose an existing security group from the list, or **Create Security Group** to use with the development endpoint\. 

1. In the security group pane, navigate to the **Inbound** tab\.

1. Add a self\-referencing rule to allow AWS Glue components to communicate\. Specifically, add or confirm that there is a rule of **Type** `All TCP`, **Protocol** is `TCP`, **Port Range** includes all ports, and whose **Source** is the same security group name as the **Group ID**\. 

   The inbound rule looks similar to this:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/start-development-endpoint.html)

   The following shows an example of a self\-referencing inbound rule:  
![\[Image showing an example of a self-referencing inbound rule.\]](http://docs.aws.amazon.com/glue/latest/dg/images/SetupSecurityGroup-Start.png)

1. Add a rule to for outbound traffic also\. Either open outbound traffic to all ports, or create a self\-referencing rule of **Type** `All TCP`, **Protocol** is `TCP`, **Port Range** includes all ports, and whose **Source** is the same security group name as the **Group ID**\. 

   The outbound rule looks similar to one of these rules:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/start-development-endpoint.html)

## Setting Up Amazon EC2 for a Notebook Server<a name="setup-vpc-for-notebook-server"></a>

 With a development endpoint, you can create a notebook server to test your ETL scripts with Zeppelin notebooks\. To enable communication to your notebook, specify a security group with inbound rules for both HTTPS \(port 443\) and SSH \(port 22\)\. Ensure that the rule's source is either 0\.0\.0\.0/0 or the IP address of the machine that is connecting to the notebook\. 

**To set up a security group**

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the left navigation pane, choose **Security Groups**\.

1. Either choose an existing security group from the list, or **Create Security Group** to use with your notebook server\. The security group that is associated with your development endpoint is also used to create your notebook server\.

1. In the security group pane, navigate to the **Inbound** tab\.

1. Add inbound rules similar to this:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/start-development-endpoint.html)

   The following shows an example of the inbound rules for the security group:  
![\[Image showing an example of the inbound rules for the security group.\]](http://docs.aws.amazon.com/glue/latest/dg/images/SetupSecurityGroupNotebook-Start.png)