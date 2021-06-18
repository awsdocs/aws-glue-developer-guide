# Cross\-Account Cross\-Region Access to DynamoDB Tables<a name="aws-glue-programming-etl-dynamo-db-cross-account"></a>

AWS Glue ETL jobs support both cross\-region and cross\-account access to DynamoDB tables\. AWS Glue ETL jobs support both reading data from another AWS Account's DynamoDB table, and writing data into another AWS Account's DynamoDB table\. AWS Glue also supports both reading from a DynamoDB table in another region, and writing into a DynamoDB table in another region\. This section gives instructions on setting up the access, and provides an example script\. 

The procedures in this section reference an IAM tutorial for creating an IAM role and granting access to the role\. The tutorial also discusses assuming a role, but here you will instead use a job script to assume the role in AWS Glue\. This tutorial also contains information about general cross\-account practices\. For more information, see [Tutorial: Delegate Access Across AWS Accounts Using IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html) in the *IAM User Guide*\.

## Create a Role<a name="aws-glue-programming-etl-dynamo-db-create-role"></a>

Follow [step 1 in the tutorial](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html#tutorial_cross-account-with-roles-1) to create an IAM role in account A\. When defining the permissions of the role, you can choose to attach existing policies such as `AmazonDynamoDBReadOnlyAccess`, or `AmazonDynamoDBFullAccess` to allow the role to read/write DynamoDB\. The following example shows creating a role named `DynamoDBCrossAccessRole`, with the permission policy `AmazonDynamoDBFullAccess`\.

## Grant Access to the Role<a name="aws-glue-programming-etl-dynamo-db-grant-access"></a>

Follow [step 2 in the tutorial](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html#tutorial_cross-account-with-roles-2) in the *IAM User Guide* to allow account B to switch to the newly\-created role\. The following example creates a new policy with the following statement:

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": "sts:AssumeRole",
        "Resource": "<DynamoDBCrossAccessRole's ARN>"
    }
}
```

Then, you can attach this policy to the group/role/user you would like to use to access DynamoDB\.

## Assume the Role in the AWS Glue Job Script<a name="aws-glue-programming-etl-dynamo-db-assume-role"></a>

Now, you can log in to account B and create an AWS Glue job\. To create a job, refer to the instructions at [Adding Jobs in AWS Glue](add-job.md)\. 

In the job script you need to use the `dynamodb.sts.roleArn` parameter to assume the `DynamoDBCrossAccessRole` role\. Assuming this role allows you to get the temporary credentials, which need to be used to access DynamoDB in account B\. Review these example scripts\.

For a cross\-account read across regions:

```
import sys
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job
from awsglue.utils import getResolvedOptions
 
args = getResolvedOptions(sys.argv, ["JOB_NAME"])
glue_context= GlueContext(SparkContext.getOrCreate())
job = Job(glue_context)
job.init(args["JOB_NAME"], args)
 
dyf = glue_context.create_dynamic_frame_from_options(
    connection_type="dynamodb",
    connection_options={
        "dynamodb.region": "us-east-1",
        "dynamodb.input.tableName": "test_source",
        "dynamodb.sts.roleArn": "<DynamoDBCrossAccessRole's ARN>"
    }
)
dyf.show()
job.commit()
```

For a read and cross\-account write across regions:

```
import sys
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job
from awsglue.utils import getResolvedOptions
 
args = getResolvedOptions(sys.argv, ["JOB_NAME"])
glue_context= GlueContext(SparkContext.getOrCreate())
job = Job(glue_context)
job.init(args["JOB_NAME"], args)
 
dyf = glue_context.create_dynamic_frame_from_options(
    connection_type="dynamodb",
    connection_options={
        "dynamodb.region": "us-east-1",
        "dynamodb.input.tableName": "test_source"
    }
)
dyf.show()
 
glue_context.write_dynamic_frame_from_options(
    frame=dyf,
    connection_type="dynamodb",
    connection_options={
        "dynamodb.region": "us-west-2",
        "dynamodb.output.tableName": "test_sink",
        "dynamodb.sts.roleArn": "<DynamoDBCrossAccessRole's ARN>"
    }
)
 
job.commit()
```