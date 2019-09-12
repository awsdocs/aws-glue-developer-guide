# Getting and Setting Workflow Run Properties<a name="workflow-run-properties-code"></a>

Use workflow run properties to share and manage state among the jobs in your AWS Glue workflow\. You can set default run properties when you create the workflow\. Then, as your jobs run, they can retrieve the run property values and optionally modify them for input to jobs that are later in the workflow\. When a job modifies a run property, the new value exists only for the workflow run; the default run properties are not affected\.

The following sample Python code from an extract, transform, and load \(ETL\) job demonstrates how to get the workflow run properties\.

```
import sys
import boto3
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from awsglue.context import GlueContext
from pyspark.context import SparkContext

glue_client = boto3.client("glue")
args = getResolvedOptions(sys.argv, ['JOB_NAME','WORKFLOW_NAME', 'WORKFLOW_RUN_ID'])
workflow_name = args['WORKFLOW_NAME']
workflow_run_id = args['WORKFLOW_RUN_ID']
workflow_params = glue_client.get_workflow_run_properties(Name=workflow_name,
                                        RunId=workflow_run_id)["RunProperties"]

target_database = workflow_params['target_database']
target_s3_location = workflow_params['target_s3_location']
```

The following code continues by setting the `target_format` run property to `'csv'`\.

```
workflow_params['target_format'] = 'csv'
glue_client.put_workflow_run_properties(Name=workflow_name, RunId=workflow_run_id, RunProperties=workflow_params)
```

For more information, see the following: 
+ [GetWorkflowRunProperties Action \(Python: get\_workflow\_run\_properties\)](aws-glue-api-workflow.md#aws-glue-api-workflow-GetWorkflowRunProperties)
+ [PutWorkflowRunProperties Action \(Python: put\_workflow\_run\_properties\)](aws-glue-api-workflow.md#aws-glue-api-workflow-PutWorkflowRunProperties)