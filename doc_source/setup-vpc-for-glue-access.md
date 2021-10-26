# Setting Up a VPC to Connect to JDBC Data Stores<a name="setup-vpc-for-glue-access"></a>

To enable AWS Glue components to communicate, you must set up access to your data stores, such as Amazon Redshift and Amazon RDS\. In order for VPC Functionality to work AWS Glue needs to communicate between its components, create a security group with a self\-referencing inbound rule for all TCP ports\. By creating a self\-referencing rule, you can restrict the source to the same security group in the VPC, and it's not open to all networks\. See more about [Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-group-rules.html)\.

## ****To set up access for AWS Glue****

1. First, create a new security group for Glue to use.

1. Sign in to the AWS Management Console and open the VPC Management Console at [https://console.aws.amazon.com/vpc/](https://console.aws.amazon.com/vpc/) 

1. In the left navigation pane, choose **Security -> Security Groups**.

1. Create a new security group for Glue to use. 

1. Choose the security group and navigate to the **Inbound** tab\.

1. Add a self\-referencing rule to allow AWS Glue components to communicate between themselves\. Specifically, add or confirm that there is a rule of **Type** `All TCP`, **Protocol** is `TCP`, **Port Range** includes all ports, and whose **Source** is the same security group name as the **Group ID**\. **This is the Glue-sg**.

   The inbound rule looks similar to the following:  
   |Type|Protocol|Port Range|Source|
   |--|--|--|--|
   |All TCP|TCP|ALL|**Glue-sg**| 

1. Add a rule for outbound traffic also\. Either open outbound traffic to all ports, for example: 
 
    |Type|Protocol|Port Range|Destination|
    |--|--|--|--|
    |All Traffic|ALL|ALL|0.0.0.0/0|

   Or create a self\-referencing rule where **Type** `All TCP`, **Protocol** is `TCP`, **Port Range** includes all ports, and whose **Destination** is the same security group name as the **Group ID**\. If using an Amazon S3 VPC endpoint, also add an HTTPS rule for Amazon S3 access\. The *s3\-prefix\-list\-id* is required in the security group rule to allow traffic from the VPC to the Amazon S3 VPC endpoint\. See more [Documentation for Gateway VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-gateway.html)\.

   |Type|Protocol|Port Range|Destination|
   |--|--|--|--|
   |All TCP|TCP|ALL|**Glue-sg**|
   |HTTPS|TCP|443|**s3-prefix-list-id**|

3. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

4. In the left navigation pane, choose **Databases -> Connections**\.

5. Add or Edit a Connection

6. In the **Databases -> Connections** section, choose a security group in **VPC security groups** that was previously created as **Glue-sg**\. 

## **To set up access for Amazon Redshift data stores**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

2. In the left navigation pane, choose **Clusters**\.

3. Choose the cluster name that you want to access from AWS Glue\.

4. In the **Cluster Properties** section, choose a security group in **VPC security groups** to allow AWS Redshift to use\. Record the name of the security group that you chose for future reference\. Choosing the security group opens the Amazon EC2 console **Security Groups** list\. This is the **Redshift-sg**.

5. Choose the security group to modify and navigate to the **Inbound** tab\.

6. Add a rule to allow AWS Glue components to communicate\. Specifically, add or confirm that there is a rule of **Type** `Redshift`, **Protocol** is `TCP`, **Port Range** is defined by the choice of Type, and whose **Source** is the same security group name as the **Glue-sg** that was previously configured\.  

   The inbound rule looks similar to the following:   
   
   |Type|Protocol|Port Range|Source|
   |--|--|--|--|
   |Redshift|TCP|5439|**Glue-sg**| 


7. Add a rule for outbound traffic also\. Either open outbound traffic to all ports:                  
    |Type|Protocol|Port Range|Destination|
    |--|--|--|--|
    |All Traffic|ALL|ALL|0.0.0.0/0|

   Or if using an Amazon S3 VPC endpoint, also add an HTTPS rule for Amazon S3 access\. The s3\-prefix\-list\-id* is required in the security group rule to allow traffic from the VPC to the    Amazon S3 VPC endpoint\. See more [Documentation for Gateway VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-gateway.html)\.

   |Type|Protocol|Port Range|Destination|
   |--|--|--|--|
   |HTTPS|TCP|443|**s3-prefix-list-id**| 

# **To set up access for Amazon RDS data stores**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

2. In the left navigation pane, choose **Instances**\.

3. Choose the Amazon RDS **Engine** and **DB Instance** name that you want to access from AWS Glue\.

4. From **Instance Actions**, choose **See Details**\. On the **Details** tab, find the **Security Groups** name that is assigned to the instance\. Record the name of the security group for future reference\. This is the **RDS-sg**

5. Choose the security group to open the Amazon EC2 console\.

6. Confirm that your **Group ID** from Amazon RDS (**RDS-sg**) is chosen, then choose the **Inbound** tab\.

7. Add a rule to allow AWS Glue components to communicate\. Specifically, add or confirm that there is a rule of **Type** `MYSQL/Aurora OR PostgreSQL OR Oracle-RDS`, **Protocol** is `TCP`, **Port Range** is defined by the choice of Type, and whose **Source** is the same security group name as the **Glue-sg** that was previously configured\. 

   The inbound rule looks similar to this:  
   |Type|Protocol|Port Range|Source|
   |--|--|--|--|
   |MYSQL/Aurora|TCP|3306|**Glue-sg**|\
   
   or
   
   |Type|Protocol|Port Range|Source|
   |--|--|--|--|
   |PostgreSQL|TCP|5432|**Glue-sg**| 
   
   or
   
   |Type|Protocol|Port Range|Source|
   |--|--|--|--|
   |Oracle-RDS|TCP|1521|**Glue-sg**| 


8. Add a rule for outbound traffic also\. Either open outbound traffic to all ports:  
    |Type|Protocol|Port Range|Destination|
    |--|--|--|--|
    |All Traffic|ALL|ALL|0.0.0.0/0|
    
   Or if using an Amazon S3 VPC endpoint, also add an HTTPS rule for Amazon S3 access\. The *s3\-prefix\-list\-id* is required in the security group rule to allow traffic from the VPC to the Amazon S3 VPC endpoint\. See more [Documentation for Gateway VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-gateway.html)\.
   
   |Type|Protocol|Port Range|Destination|
   |--|--|--|--|
   |HTTPS|TCP|443|**s3-prefix-list-id**| 
