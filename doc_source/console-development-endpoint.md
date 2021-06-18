# Viewing Development Endpoint Properties<a name="console-development-endpoint"></a>

You can view development endpoint details using the AWS Glue console\. Endpoint details include the information that you defined when you created it using the **Add endpoint** wizard\. They also include information that you need to connect to the endpoint and information about any notebooks that use the endpoint\.

**To view development endpoint properties**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\. Sign in as a user who has the IAM permissions `glue:GetDevEndpoints` and `glue:GetDevEndpoint`\.

1. In the navigation pane, under **ETL**, choose **Dev endpoints**\.

1. On the **Dev endpoints** page, choose the name of the development endpoint\.

The following are some of the development endpoint properties:

**Endpoint name**  
The unique name that you give the endpoint when you create it\.

**Provisioning status**  
Describes whether the endpoint is being created \(**PROVISIONING**\), ready to be used \(**READY**\), in the process of terminating \(**TERMINATING**\), terminated \(**TERMINATED**\), or failed \(**FAILED**\)\. 

**Failure reason**  
The reason for a development endpoint failure\.

**Private address**  
The address to connect to the development endpoint\. On the Amazon EC2 console, you can view the elastic network interface that is attached to this IP address\. This internal address is created if the development endpoint is associated with a virtual private cloud \(VPC\)\.   
For more information about accessing a development endpoint from a private address, see [Accessing Your Development Endpoint](dev-endpoint-elastic-ip.md)\.

**Public address**  
The address to connect to the development endpoint\. 

**Public key contents**  
The current public SSH keys that are associated with the development endpoint \(optional\)\. If you provided a public key when you created the development endpoint, you should have saved the corresponding SSH private key\.

**IAM role**  
Specify the IAM role that is used for authorization to resources\. If the development endpoint reads AWS KMS encrypted Amazon S3 data, the **IAM role** must have decrypt permission on the KMS key\.   
For more information about creating an IAM role, see [Step 2: Create an IAM Role for AWS Glue](create-an-iam-role.md)\.

**SSH to Python REPL**  
You can open a terminal window on your computer and enter this command to interact with the development endpoint as a read\-eval\-print loop \(REPL\) shell\. This field is shown only if the development endpoint contains a public SSH key\.

**SSH to Scala REPL**  
You can open a terminal window and enter this command to interact with the development endpoint as a REPL shell\. This field is shown only if the development endpoint contains a public SSH key\.

**SSH tunnel to remote interpreter**  
You can open a terminal window and enter this command to open a tunnel to the development endpoint\. Then open your local Apache Zeppelin notebook and point to the development endpoint as a remote interpreter\. When the interpreter is set up, all notes within the notebook can use it\. This field is shown only if the development endpoint contains a public SSH key\.

**Public key update status**  
The status of completing an update of the public key on the development endpoint\. When you update a public key, the new key must be propagated to the development endpoint\. Status values include `COMPLETED` and `PENDING`\.

**Last modified time**  
The last time this development endpoint was modified\.

**Running for**  
The amount of time the development endpoint has been provisioned and `READY`\.