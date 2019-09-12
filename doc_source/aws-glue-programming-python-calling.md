# Calling AWS Glue APIs in Python<a name="aws-glue-programming-python-calling"></a>

Note that Boto 3 resource APIs are not yet available for AWS Glue\. Currently, only the Boto 3 client APIs can be used\.

## AWS Glue API Names in Python<a name="aws-glue-programming-python-calling-names"></a>

AWS Glue API names in Java and other programming languages are generally CamelCased\. However, when called from Python, these generic names are changed to lowercase, with the parts of the name separated by underscore characters to make them more "Pythonic"\. In the [AWS Glue API](aws-glue-api.md) reference documentation, these Pythonic names are listed in parentheses after the generic CamelCased names\.

However, although the AWS Glue API names themselves are transformed to lowercase, their parameter names remain capitalized\. It is important to remember this, because parameters should be passed by name when calling AWS Glue APIs, as described in the following section\.

## Passing and Accessing Python Parameters in AWS Glue<a name="aws-glue-programming-python-calling-parameters"></a>

In Python calls to AWS Glue APIs, it's best to pass parameters explicitly by name\. For example:

```
job = glue.create_job(Name='sample', Role='Glue_DefaultRole',
                      Command={'Name': 'glueetl',
                               'ScriptLocation': 's3://my_script_bucket/scripts/my_etl_script.py'})
```

It is helpful to understand that Python creates a dictionary of the name/value tuples that you specify as arguments to an ETL script in a [Job Structure](aws-glue-api-jobs-job.md#aws-glue-api-jobs-job-Job) or [JobRun Structure](aws-glue-api-jobs-runs.md#aws-glue-api-jobs-runs-JobRun)\. Boto 3 then passes them to AWS Glue in JSON format by way of a REST API call\. This means that you cannot rely on the order of the arguments when you access them in your script\.

For example, suppose that you're starting a `JobRun` in a Python Lambda handler function, and you want to specify several parameters\. Your code might look something like the following:

```
from datetime import datetime, timedelta

client = boto3.client('glue')

def lambda_handler(event, context):
  last_hour_date_time = datetime.now() - timedelta(hours = 1)
  day_partition_value = last_hour_date_time.strftime("%Y-%m-%d")
  hour_partition_value = last_hour_date_time.strftime("%-H")

  response = client.start_job_run(
               JobName = 'my_test_Job',
               Arguments = {
                 '--day_partition_key':   'partition_0',
                 '--hour_partition_key':  'partition_1',
                 '--day_partition_value':  day_partition_value,
                 '--hour_partition_value': hour_partition_value } )
```

To access these parameters reliably in your ETL script, specify them by name using AWS Glue's `getResolvedOptions` function and then access them from the resulting dictionary:

```
import sys
from awsglue.utils import getResolvedOptions

args = getResolvedOptions(sys.argv,
                          ['JOB_NAME',
                           'day_partition_key',
                           'hour_partition_key',
                           'day_partition_value',
                           'hour_partition_value'])
print "The day partition key is: ", args['day_partition_key']
print "and the day partition value is: ", args['day_partition_value']
```

## Example: Create and Run a Job<a name="aws-glue-programming-python-calling-example"></a>

The following example shows how call the AWS Glue APIs using Python, to create and run an ETL job\.

**To create and run a job**

1. Create an instance of the AWS Glue client:

   ```
   import boto3
   glue = boto3.client(service_name='glue', region_name='us-east-1',
                 endpoint_url='https://glue.us-east-1.amazonaws.com')
   ```

1. Create a job\. You must use `glueetl` as the name for the ETL command, as shown in the following code:

   ```
   myJob = glue.create_job(Name='sample', Role='Glue_DefaultRole',
                             Command={'Name': 'glueetl',
                                      'ScriptLocation': 's3://my_script_bucket/scripts/my_etl_script.py'})
   ```

1. Start a new run of the job that you created in the previous step:

   ```
   myNewJobRun = glue.start_job_run(JobName=myJob['Name'])
   ```

1. Get the job status:

   ```
   status = glue.get_job_run(JobName=myJob['Name'], RunId=JobRun['JobRunId'])
   ```

1. Print the current state of the job run:

   ```
   print status['JobRun']['JobRunState']
   ```