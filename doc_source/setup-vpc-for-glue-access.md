# Setting Up a VPC to Connect to JDBC Data Stores<a name="setup-vpc-for-glue-access"></a>

To enable AWS Glue components to communicate, you must set up access to your data stores, such as Amazon Redshift and Amazon RDS\. To enable AWS Glue to communicate between its components, specify a security group with a self\-referencing inbound rule for all TCP ports\. By creating a self\-referencing rule, you can restrict the source to the same security group in the VPC, and it's not open to all networks\. The default security group for your VPC might already have a self\-referencing inbound rule for ALL Traffic\. 

**To set up access for Amazon Redshift data stores**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the left navigation pane, choose **Clusters**\.

1. Choose the cluster name that you want to access from AWS Glue\.

1. In the **Cluster Properties** section, choose a security group in **VPC security groups** to allow AWS Glue to use\. Record the name of the security group that you chose for future reference\. Choosing the security group opens the Amazon EC2 console **Security Groups** list\.

1. Choose the security group to modify and navigate to the **Inbound** tab\.

1. Add a self\-referencing rule to allow AWS Glue components to communicate\. Specifically, add or confirm that there is a rule of **Type** `All TCP`, **Protocol** is `TCP`, **Port Range** includes all ports, and whose **Source** is the same security group name as the **Group ID**\. 

   The inbound rule looks similar to the following:   
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/setup-vpc-for-glue-access.html)

   For example:  
![\[An example of a self-referencing inbound rule.\]](http://docs.aws.amazon.com/glue/latest/dg/images/SetupSecurityGroup-Start.png)

1. Add a rule for outbound traffic also\. Either open outbound traffic to all ports, for example:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/setup-vpc-for-glue-access.html)

   Or create a self\-referencing rule where **Type** `All TCP`, **Protocol** is `TCP`, **Port Range** includes all ports, and whose **Destination** is the same security group name as the **Group ID**\. If using an Amazon S3 VPC endpoint, also add an HTTPS rule for Amazon S3 access\. The *s3\-prefix\-list\-id* is required in the security group rule to allow traffic from the VPC to the Amazon S3 VPC endpoint\.

   For example:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/setup-vpc-for-glue-access.html)

**To set up access for Amazon RDS data stores**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the left navigation pane, choose **Instances**\.

1. Choose the Amazon RDS **Engine** and **DB Instance** name that you want to access from AWS Glue\.

1. From **Instance Actions**, choose **See Details**\. On the **Details** tab, find the **Security Groups** name you will access from AWS Glue\. Record the name of the security group for future reference\.

1. Choose the security group to open the Amazon EC2 console\.

1. Confirm that your **Group ID** from Amazon RDS is chosen, then choose the **Inbound** tab\.

1. Add a self\-referencing rule to allow AWS Glue components to communicate\. Specifically, add or confirm that there is a rule of **Type** `All TCP`, **Protocol** is `TCP`, **Port Range** includes all ports, and whose **Source** is the same security group name as the **Group ID**\. 

   The inbound rule looks similar to this:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/setup-vpc-for-glue-access.html)

   For example:  
![\[An example of a self-referencing inbound rule.\]](http://docs.aws.amazon.com/glue/latest/dg/images/SetupSecurityGroup-Start.png)

1. Add a rule for outbound traffic also\. Either open outbound traffic to all ports, for example:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/setup-vpc-for-glue-access.html)

   Or create a self\-referencing rule where **Type** `All TCP`, **Protocol** is `TCP`, **Port Range** includes all ports, and whose **Destination** is the same security group name as the **Group ID**\. If using an Amazon S3 VPC endpoint, also add an HTTPS rule for Amazon S3 access\. The *s3\-prefix\-list\-id* is required in the security group rule to allow traffic from the VPC to the Amazon S3 VPC endpoint\.

   For example:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/setup-vpc-for-glue-access.html)