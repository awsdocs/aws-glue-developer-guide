# Prerequisites for Developing Blueprints<a name="developing-blueprints-prereq"></a>

To develop blueprints, you should be familiar with using AWS Glue and writing scripts for Apache Spark ETL jobs or Python shell jobs\. In addition, you must complete the following setup tasks\. Some of the tasks are just for the public preview\.
+ Download four AWS Python libraries to use in your blueprint layout scripts\.
+ Set up the AWS SDKs for the public preview\.
+ Set up the preview AWS CLI\.

## Download the Python Libraries<a name="prereqs-get-libes"></a>

Download the following libraries from GitHub, and install them into your project:
+ [https://github\.com/awslabs/aws\-glue\-blueprint\-libs/tree/master/awsglue/blueprint/base\_resource\.py](https://github.com/awslabs/aws-glue-blueprint-libs/tree/master/awsglue/blueprint/base_resource.py)
+ [https://github\.com/awslabs/aws\-glue\-blueprint\-libs/tree/master/awsglue/blueprint/workflow\.py](https://github.com/awslabs/aws-glue-blueprint-libs/tree/master/awsglue/blueprint/workflow.py)
+ [https://github\.com/awslabs/aws\-glue\-blueprint\-libs/tree/master/awsglue/blueprint/crawler\.py](https://github.com/awslabs/aws-glue-blueprint-libs/tree/master/awsglue/blueprint/crawler.py)
+ [https://github\.com/awslabs/aws\-glue\-blueprint\-libs/tree/master/awsglue/blueprint/job\.py](https://github.com/awslabs/aws-glue-blueprint-libs/tree/master/awsglue/blueprint/job.py)

## Set Up the AWS Java SDK for the Public Preview<a name="prereqs-java-preview-sdk"></a>

For the AWS Java SDK, you must add a `jar` file that includes the API for blueprints\.

1. If you haven't already done so, set up the AWS SDK for Java\.
   + For Java 1\.x, follow the instructions in [Set up the AWS SDK for Java](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-install.html) in the *AWS SDK for Java Developer Guide*\.
   + For Java 2\.x, follow the instructions in [Setting up the AWS SDK for Java 2\.x](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/setup.html) in the *AWS SDK for Java 2\.x Developer Guide*\.

1. Download the preview client `jar` file that has access to the APIs for blueprints\.
   + For Java 1\.x: s3://awsglue\-custom\-blueprints\-preview\-artifacts/awsglue\-java\-sdk\-preview/AWSGlueJavaClient\-1\.11\.x\.jar
   + For Java 2\.x: s3://awsglue\-custom\-blueprints\-preview\-artifacts/awsglue\-java\-sdk\-v2\-preview/AwsJavaSdk\-Glue\-2\.0\.jar

1. Add the preview client `jar` to the front of the Java classpath to override the AWS Glue client provided by the AWS Java SDK\.

   ```
   export CLASSPATH=<path-to-preview-client-jar>:$CLASSPATH
   ```

1. \(Optional\) Test the preview SDK with the following Java application\. The application should output an empty list\.

   Replace `accessKey` and `secretKey` with your credentials, and replace `us-east-1` with your Region\.

   ```
   import com.amazonaws.auth.AWSCredentials;
   import com.amazonaws.auth.AWSCredentialsProvider;
   import com.amazonaws.auth.AWSStaticCredentialsProvider;
   import com.amazonaws.auth.BasicAWSCredentials;
   import com.amazonaws.services.glue.AWSGlue;
   import com.amazonaws.services.glue.AWSGlueClientBuilder;
   import com.amazonaws.services.glue.model.ListBlueprintsRequest;
   
   public class App{
       public static void main(String[] args) {
           AWSCredentials credentials = new BasicAWSCredentials("accessKey", "secretKey");
           AWSCredentialsProvider provider = new AWSStaticCredentialsProvider(credentials);
           AWSGlue glue = AWSGlueClientBuilder.standard().withCredentials(provider)
                   .withRegion("us-east-1").build();
           ListBlueprintsRequest request = new ListBlueprintsRequest().withMaxResults(2);
           System.out.println(glue.listBlueprints(request));
       }
   }
   ```

## Set Up the AWS Python SDK for the Public Preview<a name="prereqs-python-preview-sdk"></a>

The following steps assume that you have Python version 2\.7 or later, or version 3\.6 or later installed on your computer\.

1. Download the following boto3 wheel file\. If prompted to open or save, save the file\. s3://awsglue\-custom\-blueprints\-preview\-artifacts/aws\-python\-sdk\-preview/boto3\-1\.17\.31\-py2\.py3\-none\-any\.whl

1. Download the following botocore wheel file: s3://awsglue\-custom\-blueprints\-preview\-artifacts/aws\-python\-sdk\-preview/botocore\-1\.20\.31\-py2\.py3\-none\-any\.whl

1. Check your Python version\.

   ```
   python --version
   ```

1. Depending on your Python version, enter the following commands \(for Linux\):
   + For Python 2\.7 or later\.

     ```
     python3 -m pip install --user virtualenv
     source env/bin/activate
     ```
   + For Python 3\.6 or later\.

     ```
     python3 -m venv python-sdk-test
     source python-sdk-test/bin/activate
     ```

1. Install the botocore wheel file\.

   ```
   python3 -m pip install <download-directory>/botocore-1.20.31-py2.py3-none-any.whl
   ```

1. Install the boto3 wheel file\.

   ```
   python3 -m pip install <download-directory>/boto3-1.17.31-py2.py3-none-any.whl
   ```

1. Configure your credentials and default region in the `~/.aws/credentials` and `~/.aws/config` files\. For more information, see [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) in the *AWS Command Line Interface User Guide*\.

1. \(Optional\) Test your setup\. The following commands should return an empty list\.

   Replace `us-east-1` with your Region\.

   ```
   $ python
   >>> import boto3
   >>> glue = boto3.client('glue', 'us-east-1')
   >>> glue.list_blueprints()
   ```

## Set Up the Preview AWS CLI<a name="prereqs-setup-cli"></a>

1. If you haven't already done so, install and/or update the AWS Command Line Interface \(AWS CLI\) on your computer\. The easiest way to do this is with `pip`, the Python installer utility:

   ```
   pip install awscli --upgrade --user
   ```

   You can find complete installation instructions for the AWS CLI here: [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

1. Download the AWS CLI wheel file from: s3://awsglue\-custom\-blueprints\-preview\-artifacts/awscli\-preview\-build/awscli\-1\.19\.31\-py2\.py3\-none\-any\.whl

1. Install the AWS CLI wheel file\.

   ```
   python3 -m pip install awscli-1.19.31-py2.py3-none-any.whl
   ```

1. Run the `aws configure` command\. Configure your AWS credentials \(including access key, and secret key\) and AWS Region\. You can find information on configuring the AWS CLI here: [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)\.

1. Test the AWS CLI\. The following command should return an empty list\.

   Replace `us-east-1` with your Region\.

   ```
   aws glue list-blueprints --region us-east-1
   ```