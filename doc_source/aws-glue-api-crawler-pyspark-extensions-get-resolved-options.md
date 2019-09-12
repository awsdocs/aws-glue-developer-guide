# Accessing Parameters Using `getResolvedOptions`<a name="aws-glue-api-crawler-pyspark-extensions-get-resolved-options"></a>

The AWS Glue `getResolvedOptions(args, options)` utility function gives you access to the arguments that are passed to your script when you run a job\. To use this function, start by importing it from the AWS Glue `utils` module, along with the `sys` module:

```
import sys
from awsglue.utils import getResolvedOptions
```

**`getResolvedOptions(args, options)`**
+ `args` – The list of arguments contained in `sys.argv`\.
+ `options` – A Python array of the argument names that you want to retrieve\.

**Example Retrieving arguments passed to a JobRun**  
Suppose that you created a JobRun in a script, perhaps within a Lambda function:  

```
response = client.start_job_run(
             JobName = 'my_test_Job',
             Arguments = {
               '--day_partition_key':   'partition_0',
               '--hour_partition_key':  'partition_1',
               '--day_partition_value':  day_partition_value,
               '--hour_partition_value': hour_partition_value } )
```
To retrieve the arguments that are passed, you can use the `getResolvedOptions` function as follows:  

```
import sys
from awsglue.utils import getResolvedOptions

args = getResolvedOptions(sys.argv,
                          ['JOB_NAME',
                           'day_partition_key',
                           'hour_partition_key',
                           'day_partition_value',
                           'hour_partition_value'])
print "The day-partition key is: ", args['day_partition_key']
print "and the day-partition value is: ", args['day_partition_value']
```
Note that each of the arguments are defined as beginning with two hyphens, then referenced in the script without the hyphens\. Your arguments need to follow this convention to be resolved\.