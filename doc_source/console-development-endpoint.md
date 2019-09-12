# Working with Development Endpoints on the AWS Glue Console<a name="console-development-endpoint"></a>

A development endpoint is an environment that you can use to develop and test your AWS Glue scripts\. The **Dev endpoints** tab on the AWS Glue console lists all the development endpoints that you have created\. You can add, delete, or rotate the SSH key of a development endpoint\. You can also create notebooks that use the development endpoint\.

To display details for a development endpoint, choose the endpoint in the list\. Endpoint details include the information you defined when you created it using the **Add endpoint** wizard\. They also include information that you need to connect to the endpoint and any notebooks that use the endpoint\.

Follow the instructions in the tutorial topics to learn the details about how to use your development endpoint with notebooks\.

The following are some of the development endpoint properties:

**Endpoint name**  
The unique name that you give the endpoint when you create it\.

**Provisioning status**  
Describes whether the endpoint is being created \(**PROVISIONING**\), ready to be used \(**READY**\), in the process of terminating \(**TERMINATING**\), terminated \(**TERMINATED**\), or failed \(**FAILED**\)\. 

**Failure reason**  
Reason for the development endpoint failure\.

**Private address**  
Address to connect to the development endpoint\. On the Amazon EC2 console, you can view the ENI attached to this IP address\. This internal address is created if the development endpoint is associated with a virtual private cloud \(VPC\)\. For more information about accessing a development endpoint from a private address, see [Accessing Your Development Endpoint](dev-endpoint.md#dev-endpoint-elastic-ip)\.

**Public address**  
Address to connect to the development endpoint\. 

**Public key contents**  
Current public SSH keys that are associated with the development endpoint \(optional\)\. If you provided a public key when you created the development endpoint, you should have saved the corresponding SSH private key\.

**IAM role**  
Specify the IAM role that is used for authorization to resources\. If the development endpoint reads KMS encrypted Amazon S3 data, then the **IAM role** must have decrypt permission on the KMS key\. For more information about creating an IAM role, see [Step 2: Create an IAM Role for AWS Glue](create-an-iam-role.md)\.

**SSH to Python REPL**  
You can open a terminal window on your computer \(laptop\) and type this command to interact with the development endpoint in as a Read\-Eval\-Print Loop \(REPL\) shell\. This field is only shown if the development endpoint contains a public SSH key\.

**SSH to Scala REPL**  
You can open a terminal window on your computer and type this command to interact with the development endpoint as a REPL shell\. This field is only shown if the development endpoint contains a public SSH key\.

**SSH tunnel to remote interpreter**  
You can open a terminal window on your computer and type this command to open a tunnel to the development endpoint\. Then open your local Apache Zeppelin notebook and point to the development endpoint as a remote interpreter\. Once the interpreter is set up, all notes within the notebook can use it\. This field is only shown if the development endpoint contains a public SSH key\.

**Public key update status**  
The status of completing an update of the public key on the development endpoint\. When you update a public key, the new key must be propagated to the development endpoint\. Status values include `COMPLETED` and `PENDING`\.

**Last modified time**  
Last time this development endpoint was modified\.

**Running for**  
Amount of time the development endpoint has been provisioned and `READY`\.

## Adding an Endpoint<a name="console-endpoint-wizard"></a>

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. Choose the **Dev endpoints** tab, and then choose **Add endpoint**\.

1. Follow the steps in the AWS Glue **Add endpoint** wizard to provide the properties that are required to create an endpoint\. If you choose to provide an SSH public key when you create your development endpoint, save your SSH private key to access the development endpoint later\. 

   The following are some optional fields you can provide:  
**Security configuration**  
Add a security configuration to a development endpoint to specify at\-rest encryption options\.   
**Data processing units \(DPUs\)**  
The number of DPUs that AWS Glue uses for your development endpoint\. The number must be greater than 1\.  
**Python library path**  
Comma\-separated Amazon Simple Storage Service \(Amazon S3\) paths to Python libraries that are required by your script\. Multiple values must be complete paths separated by a comma \(`,`\)\. Only individual files are supported, not a directory path\.  
Only pure Python libraries can be used\. Libraries that rely on C extensions, such as the pandas Python Data Analysis Library, are not yet supported\.  
**Dependent jars path**  
Comma\-separated Amazon S3 paths to JAR files that are required by the script\.  
Currently, only pure Java or Scala \(2\.11\) libraries can be used\.  
**Tags**  
Tag your development endpoint with a **Tag key** and optional **Tag value**\. Once created, tag keys are read\-only\. Use tags on some resources to help you organize and identify them\. For more information, see [AWS Tags in AWS Glue](monitor-tags.md)\.   
**Use Glue Data Catalog as the Hive metastore**  
Enables you to use the AWS Glue Data Catalog as a Spark Hive metastore\.  
**Worker type**  
The type of predefined worker that is allocated to the development endpoint\. Accepts a value of Standard, G\.1X, or G\.2X\.  
   + For the `Standard` worker type, each worker provides 4 vCPU, 16 GB of memory and a 50GB disk, and 2 executors per worker\.
   + For the `G.1X` worker type, each worker maps to 1 DPU \(4 vCPU, 16 GB of memory, 64 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.
   + For the `G.2X` worker type, each worker maps to 2 DPU \(8 vCPU, 32 GB of memory, 128 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.
Known issue: when a development endpoint is created with the `G.2X` `WorkerType` configuration, the Spark drivers for the development endpoint will run on 4 vCPU, 16 GB of memory, and a 64 GB disk\.   
**Number of workers**  
The number of workers of a defined `workerType` that are allocated to the development endpoint\.  
The maximum number of workers you can define are 299 for `G.1X`, and 149 for `G.2X`\. 

## Creating a Notebook Server Hosted on Amazon EC2<a name="console-ec2-notebook-create"></a>

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