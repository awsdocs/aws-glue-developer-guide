# Setting Up a VPC to Connect to JDBC Data Stores<a name="setup-vpc-for-glue-access"></a>

To enable AWS Glue components to communicate, you must set up access to your data stores, such as Amazon Redshift and Amazon RDS\. In order for VPC Functionality to work AWS Glue needs to communicate between its components, create a security group with a self\-referencing inbound rule for all TCP ports\. By creating a self\-referencing rule, you can restrict the source to the same security group in the VPC, and it's not open to all networks\.

## ****To set up access for AWS Glue****

1. First, create a new security group for Glue to use.

2. Sign in to the AWS Management Console and open the VPC Management Console at [https://console.aws.amazon.com/vpc/](https://console.aws.amazon.com/vpc/) 

3. In the left navigation pane, choose **Security -> Security Groups**.

4. Create a new security group for glue to use. 

5. Choose the security group and navigate to the **Inbound** tab\.

6. Add a self\-referencing rule to allow AWS Glue components to communicate between themselves\. Specifically, add or confirm that there is a rule of **Type** `All TCP`, **Protocol** is `TCP`, **Port Range** includes all ports, and whose **Source** is the same security group name as the **Group ID**\. **This is the Glue-sg**.

  The inbound rule looks similar to the following:   
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/setup-vpc-for-glue-access.html)

   For example:  
![\[An example of a self-referencing inbound rule.\]](http://docs.aws.amazon.com/glue/latest/dg/images/SetupSecurityGroup-Start.png)

1. Add a rule for outbound traffic also\. Either open outbound traffic to all ports, for example: 
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/setup-vpc-for-glue-access.html)

   Or create a self\-referencing rule where **Type** `All TCP`, **Protocol** is `TCP`, **Port Range** includes all ports, and whose **Destination** is the same security group name as the **Group ID**\. If using an Amazon S3 VPC endpoint, also add an HTTPS rule for Amazon S3 access\. The *s3\-prefix\-list\-id* is required in the security group rule to allow traffic from the VPC to the Amazon S3 VPC endpoint\.

7. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

8. In the left navigation pane, choose **Databases -> Connections**\.

9. Add or Edit a Connection

10. In the **Databases -> Connections** section, choose a security group in **VPC security groups** that was previously create as **Glue-sg**\. 


## **To set up access for Amazon Redshift data stores**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the left navigation pane, choose **Clusters**\.

1. Choose the cluster name that you want to access from AWS Glue\.

1. In the **Cluster Properties** section, choose a security group in **VPC security groups** to allow AWS Redshift to use\. Record the name of the security group that you chose for future reference\. Choosing the security group opens the Amazon EC2 console **Security Groups** list\. This is the **Redshift-sg**.

1. Choose the security group to modify and navigate to the **Inbound** tab\.

1. Add a rule to allow AWS Glue components to communicate\. Specifically, add or confirm that there is a rule of **Type** `Redshift`, **Protocol** is `TCP`, **Port Range** is defined by the choice of Type, and whose **Source** is the same security group name as the **Glue-sg** that was previously configured\.  

   The inbound rule looks similar to the following:   
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/setup-vpc-for-glue-access.html)

   For example:  
![\[An example of a self-referencing inbound rule.\]](http://docs.aws.amazon.com/glue/latest/dg/images/SetupSecurityGroup-Start.png)

1. Add a rule for outbound traffic also\. Either open outbound traffic to all ports:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/setup-vpc-for-glue-access.html)

Or

  If using an Amazon S3 VPC endpoint, also add an HTTPS rule for Amazon S3 access\. The *s3\-prefix\-list\-id* is required in the security group rule to allow traffic from the VPC to the Amazon S3 VPC endpoint\.

 

# **To set up access for Amazon RDS data stores**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

2. In the left navigation pane, choose **Instances**\.

3. Choose the Amazon RDS **Engine** and **DB Instance** name that you want to access from AWS Glue\.

4. From **Instance Actions**, choose **See Details**\. On the **Details** tab, find the **Security Groups** name that is assigned to the instance\. Record the name of the security group for future reference\. This is the **RDS-sg**

5. Choose the security group to open the Amazon EC2 console\.

6. Confirm that your **Group ID** from Amazon RDS (**RDS-sg**) is chosen, then choose the **Inbound** tab\.

7. Add a rule to allow AWS Glue components to communicate\. Specifically, add or confirm that there is a rule of **Type** `MYSQL/Aurora OR Postgres OR Oracle-RDS`, **Protocol** is `TCP`, **Port Range** is defined by the choice of Type, and whose **Source** is the same security group name as the **Glue-sg** that was previously configured\. 

   The inbound rule looks similar to this:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/setup-vpc-for-glue-access.html)

   For example:  
![\[An example of a self-referencing inbound rule.\]](http://docs.aws.amazon.com/glue/latest/dg/images/SetupSecurityGroup-Start.png)

1. Add a rule for outbound traffic also\. Either open outbound traffic to all ports, for example:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/setup-vpc-for-glue-access.html)

 If using an Amazon S3 VPC endpoint, also add an HTTPS rule for Amazon S3 access\. The *s3\-prefix\-list\-id* is required in the security group rule to allow traffic from the VPC to the Amazon S3 VPC endpoint\.
