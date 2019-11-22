# Launching the Spark History Server<a name="monitor-spark-ui-history"></a>

You can launch the Spark history server using a AWS CloudFormation template that hosts the server on an EC2 instance, or launch locally using Docker\.

**Topics**
+ [Launching the Spark History Server and Viewing the Spark UI Using AWS CloudFormation](#monitor-spark-ui-history-cfn)
+ [Launching the Spark History Server and Viewing the Spark UI Using Docker](#monitor-spark-ui-history-local)

## Launching the Spark History Server and Viewing the Spark UI Using AWS CloudFormation<a name="monitor-spark-ui-history-cfn"></a>

You can use an AWS CloudFormation template to start the Apache Spark history server and view the Spark web UI\. These templates are samples that you should modify to meet your requirements\.

**To start the Spark history server and view the Spark UI using AWS CloudFormation**

1. Choose one of the **Launch Stack** buttons in the following table\. This launches the stack on the AWS CloudFormation console\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/monitor-spark-ui-history.html)

1. On the **Specify template** page, choose **Next**\.

1. On the **Specify stack details** page, enter the **Stack name**\. Choose **Parameters**, and then choose **Next**\.

   1. 

**Spark UI Configuration**

      Provide the following information:
      + **IP address range** — The IP address range that can be used to view the Spark UI\. If you want to restrict access from a specific IP address range, you should use a custom value\. 
      + **History server port** — The port for the Spark UI\. You can use the default value\.
      + **Event log directory** — Choose the location where Spark event logs are stored from the AWS Glue job or development endpoints\. You must use **s3a://** for the event logs path scheme\.
      + **Spark package location** — You can use the default value\.
      + **Keystore path** — SSL/TLS keystore path for HTTPS\. If you want to use a custom keystore file, you can specify the S3 path `s3://path_to_your_keystore_file` here\. If you leave this parameter empty, a self\-signed certificate based keystore is generated and used\.
**Note**  
With a self\-signed certificate based keystore, each local machine that connects to the Spark UI must be configured to trust the certificate generated before connecting to the Spark UI\. Also, when the generated certificate expires, a new certificate must be generated and trusted on all local machines\. For more information about the setup, see [Self\-signed certificates](https://docs.aws.amazon.com/glue/latest/dg/dev-endpoint-notebook-server-considerations.html#dev-endpoint-notebook-server-self-signed-certificate)\. For more information, see [ Self\-signed certificate](https://en.wikipedia.org/wiki/Self-signed_certificate) in Wikipedia\. 
      + **Keystore password** — SSL/TLS keystore password for HTTPS\.

   1. 

**EC2 Instance Configuration**

      Provide the following information:
      + **Instance type** — The type of Amazon EC2 instance that hosts the Spark history server\. Because this template launches Amazon EC2 instance in your account, Amazon EC2 cost will be charged in your account separately\.
      + **Latest AMI ID** — The AMI ID of Amazon Linux 2 for the Spark history server instance\. You can use the default value\.
      + **VPC ID** — The virtual private cloud \(VPC\) ID for the Spark history server instance\. You can use any of the VPCs available in your account Using a default VPC with a [default Network ACL](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html#default-network-acl) is not recommended\. For more information, see [Default VPC and Default Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html) and [Creating a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#Create-VPC) in the *Amazon VPC User Guide*\.
      + **Subnet ID** — The ID for the Spark history server instance\. You can use any of the subnets in your VPC\. You must be able to reach the network from your client to the subnet\. If you want to access via the internet, you must use a public subnet that has the internet gateway in the route table\.

1. On the **Configure stack options** page, choose **Next**\.

1. On the **Review** page, review the template\. Select **I acknowledge that AWS CloudFormation might create IAM resources**, and then choose **Create stack**\.

1. Wait for the stack to be created\.

1. Open the **Outputs** tab\.

   1. Copy the URL of **SparkUiPublicUrl** if you are using a public subnet\.

   1. Copy the URL of **SparkUiPrivateUrl** if you are using a private subnet\.

1. Open a web browser, and paste in the URL\. This lets you access the server using HTTPS on the specified port\. Your browser may not recognize the server's certificate, in which case you have to override its protection and proceed anyway\. 

## Launching the Spark History Server and Viewing the Spark UI Using Docker<a name="monitor-spark-ui-history-local"></a>

If you prefer local access \(not to have an EC2 instance for the Apache Spark history server\), you can also use Docker to start the Apache Spark history server and view the Spark UI locally\. This Dockerfile is a sample that you should modify to meet your requirements\. 

**Prerequisites**

For information about how to install Docker on your laptop see the [Docker Engine community](https://docs.docker.com/install/)\.

**To start the Spark history server and view the Spark UI locally using Docker**

1. Download files from GitHub\.

   1. Download the Dockerfile and pom\.xml from [AWS Glue code samples](https://github.com/aws-samples/aws-glue-samples/tree/master/utilities/Spark_UI/)\.

1. Run the following commands:

   1. Replace the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` with your valid AWS credentials\.

   1. Replace the `s3a://path_to_eventlog` with your event log directory\.

      ```
      $ docker build -t glue/sparkui:latest . 
      $ docker run -itd -e SPARK_HISTORY_OPTS="$SPARK_HISTORY_OPTS -Dspark.history.fs.logDirectory=s3a://path_to_eventlog -Dspark.hadoop.fs.s3a.access.key=AWS_ACCESS_KEY_ID -Dspark.hadoop.fs.s3a.secret.key=AWS_SECRET_ACCESS_KEY" -p 18080:18080 glue/sparkui:latest "/opt/spark/bin/spark-class org.apache.spark.deploy.history.HistoryServer"
      ```

1. Open http://localhost:18080 in your browser to view the Spark UI locally\.