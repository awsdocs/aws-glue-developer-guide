# Creating a Notebook Server Hosted on Amazon EC2<a name="console-ec2-notebook-create"></a>

You can install an Apache Zeppelin Notebook on your local machine and use it to debug and test ETL scripts on a development endpoint\. Alternatively, you can host the Zeppelin Notebook server on an Amazon EC2 instance\. For more information, see [Creating a Notebook Server Associated with a Development Endpoint](dev-endpoint-notebook-server-considerations.md)\.

In the AWS Glue **Create notebook server** window, you add the properties that are required to create a notebook server to use an Apache Zeppelin notebook\.

**Note**  
For any notebook server that you create that is associated with a development endpoint, you manage it\. Therefore, if you delete the development endpoint, to delete the notebook server, you must delete the AWS CloudFormation stack on the AWS CloudFormation console\.

**Important**  
Before you can use the notebook server hosted on Amazon EC2, you must run a script on the Amazon EC2 instance that does the following actions:  
Sets the Zeppelin notebook password\.
Sets up communication between the notebook server and the development endpoint\.
Verifies or generates a Secure Sockets Layer \(SSL\) certificate to access the Zeppelin notebook\.
For more information, see [Creating a Notebook Server Associated with a Development Endpoint](dev-endpoint-notebook-server-considerations.md)\.

You provide the following properties: 

**CloudFormation stack name**  
The name of your notebook that is created in the AWS CloudFormation stack on the development endpoint\. The name is prefixed with `aws-glue-`\. This notebook runs on an Amazon EC2 instance\. The Zeppelin HTTP server is started either on public port 443 or localhost port 8080 that can be accessed with an SSH tunnel command\.

**IAM role**  
A role with a trust relationship to Amazon EC2 that matches the Amazon EC2 instance profile exactly\. Create the role in the IAM console\. Choose **Amazon EC2**, and attach a policy for the notebook, such as **AWSGlueServiceNotebookRoleDefault**\. For more information, see [Step 5: Create an IAM Role for Notebook Servers](create-an-iam-role-notebook.md)\.   
For more information about instance profiles, see [Using Instance Profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html)\.

**EC2 key pair**  
The Amazon EC2 key that is used to access the Amazon EC2 instance hosting the notebook server\. You can create a key pair on the Amazon EC2 console \([https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\)\. Save the key files for later use\.  For more information, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\. 

**Attach a public IP to the notebook server EC2 instance**  
Select this option to attach a public IP which can be used to access the notebook server from the internet\. Whether you choose a public or private **Subnet** is a factor when deciding to select this option\. In a public subnet, a notebook server requires a public IP to access the internet\. If your notebook server is in a private subnet and you do not want a public IP, don't select this option\. However, your notebook server still requires a route to the internet such as through a NAT gateway\. 

**Notebook username**  
The user name that you use to access the Zeppelin notebook\. The default is `admin`\.

**Notebook S3 path**  
The location where the state of the notebook is stored\. The Amazon S3 path to the Zeppelin notebook must follow the format: `s3://bucket-name/username`\. Subfolders cannot be included in the path\. The default is `s3://aws-glue-notebooks-account-id-region/notebook-username`\.

**Subnet**  
The available subnets that you can use with your notebook server\. An asterisk \(\*\) indicates that the subnet can be accessed from the internet\. The subnet must have a route to the internet through an internet gateway \(IGW\), NAT gateway, or VPN\.  For more information, see [Setting Up Your Environment for Development Endpoints](start-development-endpoint.md)\.

**Security groups**  
The available security groups that you can use with your notebook server\. The security group must have inbound rules for HTTPS \(port 443\) and SSH \(port 22\)\. Ensure that the rule's source is either 0\.0\.0\.0/0 or the IP address of the machine connecting to the notebook\.

**S3 AWS KMS key**  
A key used for client\-side KMS encryption of the Zeppelin notebook storage on Amazon S3\.  This field is optional\. Enable access to Amazon S3 by either choosing an AWS KMS key or choose **Enter a key ARN** and provide the Amazon Resource Name \(ARN\) for the key\. Type the ARN in the form `arn:aws:kms:region:account-id:key/key-id`\. You can also provide the ARN in the form of a key alias, such as `arn:aws:kms:region:account-id:alias/alias-name`\. 

**Custom AMI ID**  
A custom Amazon Machine Image \(AMI\) ID of an encrypted Amazon Elastic Block Storage \(EBS\) EC2 instance\. This field is optional\. Provide the AMI ID by either choosing an AMI ID or choose **Enter AMI ID** and type the custom AMI ID\. For more information about how to encrypt your notebook server storage, see [Encryption and AMI Copy](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/CopyingAMIs.html#ami-copy-encryption.html)\.

**Notebook server tags**  
The AWS CloudFormation stack is always tagged with a key **aws\-glue\-dev\-endpoint** and the value of the name of the development endpoint\. You can add more tags to the AWS CloudFormation stack\.

**EC2 instance**  
The name of Amazon EC2 instance that is created to host your notebook\. This links to the Amazon EC2 console \([https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\) where the instance is tagged with the key **aws\-glue\-dev\-endpoint** and value of the name of the development endpoint\. 

**CloudFormation stack**  
The name of the AWS CloudFormation stack used to create the notebook server\. 

**SSH to EC2 server command**  
Type this command in a terminal window to connect to the Amazon EC2 instance that is running your notebook server\. The Amazon EC2 address shown in this command is either public or private depending on whether you chose to **Attach a public IP to the notebook server EC2 instance**\.

**Copy certificate**  
Example scp command to copy the keystore required to set up the Zeppelin notebook server to the Amazon EC2 instance that hosts the notebook server\. Run the command from a terminal window in the directory where the Amazon EC2 private key is located\. The key to access the Amazon EC2 instance is the parameter to the `-i` option\. You provide the *path\-to\-keystore\-file*\. The location where the development endpoint private SSH key on the Amazon EC2 server is located is the remaining part of the command\.

**HTTPS URL**  
After completing the setup of a notebook server, type this URL in a browser to connect to your notebook using HTTPS\.