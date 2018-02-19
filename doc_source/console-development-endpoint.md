# Working with Development Endpoints on the AWS Glue Console<a name="console-development-endpoint"></a>

A development endpoint is an environment that you can use to develop and test your AWS Glue scripts\. The **Dev endpoints** tab on the AWS Glue console lists all the development endpoints that you have created\. You can add, delete, or rotate the SSH key of a development endpoint\. You can also create notebooks that use the development endpoint\.

To display details for a development endpoint, choose the endpoint in the list\. Endpoint details include the information you defined when you created it using the **Add endpoint** wizard\. They also include information that you need to connect to the endpoint and any notebooks that use the endpoint\. 

Follow the instructions in the tutorial topics to learn the details about how to use your development endpoint with notebooks\.

The following are some of the development endpoint properties:

**Endpoint name**  
The unique name that you give the endpoint when you create it\.

**Provisioning status**  
Describes whether the endpoint is being created \(**PROVISIONING**\), ready to be used \(**READY**\), in the process of terminating \(**UNHEALTHY\_TERMINATING**\), terminated \(**UNHEALTHY\_TERMINATED**\), failed \(**FAILED**\), or being updated \(**UPDATING**\)\.

**Failure reason**  
Reason for the development endpoint failure\.

**Public address**  
Address to connect to the development endpoint\.

**Public key contents**  
Current public SSH key associated with the development endpoint\.

**SSH to Python REPL**  
You can open a terminal window on your computer \(laptop\) and type this command to interact with the development endpoint in as a Read\-Eval\-Print Loop \(REPL\) shell\.

**SSH to Scala REPL**  
You can open a terminal window on your computer \(laptop\) and type this command to interact with the development endpoint in as a Read\-Eval\-Print Loop \(REPL\) shell\.

**SSH tunnel to remote interpreter**  
You can open a terminal window on your computer \(laptop\) and type this command to open a tunnel to the development endpoint\. Then you can open your local Apache Zeppelin notebook and point to the development endpoint as a remote interpreter\. Once the interpreter is set up, all notes within the notebook can use it\.

**Last modified time**  
Last time this development endpoint was modified\.

**Running for**  
Amount of time the development endpoint has been provisioned and `READY`\.

## Adding an Endpoint<a name="console-endpoint-wizard"></a>

To add an endpoint, sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\. Choose the **Dev endpoints** tab, and then choose **Add endpoint**\. 

Follow the steps in the AWS Glue **Add endpoint** wizard to provide the properties that are required to create an endpoint\. When you create your development endpoint, save your SSH private key to access the your development endpoint later\. The following are some optional fields you might provide:

**Data processing units \(DPUs\)**  
You can specify the number of DPUs AWS Glue uses for your development endpoint\.

**Python library path**  
Comma\-separated Amazon Simple Storage Service \(Amazon S3\) paths to Python libraries that are required by your script\.  
Only pure Python libraries can be used\. Libraries that rely on C extensions, such as the pandas Python Data Analysis Library, are not yet supported\.

**Dependent jars path**  
Comma\-separated Amazon S3 paths to JAR files that are required by the script\.  
Currently, only pure Java or Scala \(2\.11\) libraries can be used\.

## Creating a Notebook Hosted on Amazon EC2<a name="console-ec2-notebook-create"></a>

You can install an Apache Zeppelin Notebook on your local machine and use it to debug and test ETL scripts on a development endpoint\. Alternatively, you can host the Zeppelin Notebook on an Amazon EC2 instance\.

The AWS Glue **Create notebook server** window requests the properties required to create a notebook server to use an Apache Zeppelin notebook\.

**Note**  
For any notebook server that you create that is associated with a development endpoint, you manage it\. Therefore, if you delete the development endpoint, to delete the notebook server, you must delete the AWS CloudFormation stack on the AWS CloudFormation console\.

You provide the following properties\. 

**CloudFormation stack name**  
The name of your notebook that is created in the AWS CloudFormation stack on the development endpoint\. The name is prefixed with `aws-glue-`\. This notebook runs on an Amazon EC2 instance\. The Zeppelin HTTP server is started on port 443\. 

**IAM role**  
A role with a trust relationship to Amazon EC2 that matches the Amazon EC2 instance profile exactly\. Create the role in the IAM console, select **Amazon EC2**, and attach a policy for the notebook, such as **AWSGlueServiceNotebookRoleDefault**\. For more information, see [Step 5: Create an IAM Role for Notebooks](create-an-iam-role-notebook.md)\.   
For more information about instance profiles, see [Using Instance Profiles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html)\.

**EC2 key pair**  
The Amazon EC2 key that is used to access the notebook\. You can create a key pair on the Amazon EC2 console \([https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\)\.  For more information, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\. 

**SSH private key to access development endpoint**  
The private key used to connect to the development endpoint associated with the notebook server\. This private key corresponds to the current SSH public key of the development endpoint\.

**Notebook username**  
The user name that you use to access the Zeppelin notebook\.

**Notebook password**  
The password that you use to access the Zeppelin notebook\. It must not contain characters interpreted by the operating environment, such as, quotes and backticks\. 

**Notebook S3 path**  
The location where the state of the notebook is stored\. The Amazon S3 path to the Zeppelin notebook must follow the format: `s3://bucket-name/username`\. Subfolders cannot be included in the path\.

**Subnet**  
The available subnets that you can use with your notebook server\. An asterisk \(\*\) indicates that the subnet can be accessed from the internet\. The subnet must have an internet gateway \(igw\) in its route table so that it can be reached\. For more information, see [Setting Up Your Environment for Development Endpoints](start-development-endpoint.md)\.

**Security groups**  
The available security groups that you can use with your notebook server\. The security group must have inbound rules for HTTPS \(port 443\) and SSH \(port 22\)\. Ensure that the rule's source is either 0\.0\.0\.0/0 or the IP address of the machine connecting to the notebook\.

**Notebook server tags**  
The AWS CloudFormation stack is always tagged with a key **aws\-glue\-dev\-endpoint** and the value of the name of the development endpoint\. You can add more tags to the AWS CloudFormation stack\.

The AWS Glue **Development endpoints** details window displays a section for each notebook created on the development endpoint\. The following properties are shown\. 

**EC instance**  
The name of Amazon EC2 instance that is created to host your notebook\. This links to the Amazon EC2 console \([https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\) where the instance is tagged with the key **aws\-glue\-dev\-endpoint** and value of the name of the development endpoint\. 

**SSH to EC2 server command**  
Type this command in a terminal window to connect to the Amazon EC2 instance that is running your notebook\. 

**Notebook Server URL**  
Type this URL in a browser to connect to your notebook on a local port\.

**CloudFormation stack**  
The name of the AWS CloudFormation stack used to create the notebook server\. 