# AWS Glue Machine Learning Exceptions<a name="exceptions-machine-learning"></a>

This topic describes HTTP error codes and strings for AWS Glue exceptions related to machine learning\. The error codes and error strings are provided for each machine learning activity that may occur when you perform an operation\. Also, you can see whether it is possible to retry the operation that resulted in the error\.

## CancelMLTaskRunActivity<a name="exceptions-machine-learning-CancelMLTaskRunActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”
  + “No ML Task Run found for \[taskRunId\]: in account \[accountId\] for transform \[transformName\]\.”

  OK to retry: No\.

## CreateMLTaskRunActivity<a name="exceptions-machine-learning-CreateMLTransformActivity"></a>

This activity has the following exceptions:
+ InvalidInputException \(400\)
  + “Internal service failure due to unexpected input\.”
  + “An AWS Glue Table input source should be specified in transform\.”
  + “Input source column \[columnName\] has an invalid data type defined in the catalog\.”
  + “Exactly one input record table must be provided\.”
  + “Should specify database name\.”
  + “Should specify table name\.”
  + “Schema is not defined on the transform\.”
  + “Schema should contain given primary key: \[primaryKey\]\.”
  + “Problem fetching the data catalog schema: \[message\]\.”
  + “Cannot set Max Capacity and Worker Num/Type at the same time\.”
  + “Both WorkerType and NumberOfWorkers should be set\.”
  + “MaxCapacity should be >= \[maxCapacity\]\.”
  + “NumberOfWorkers should be >= \[maxCapacity\]\.”
  + “Max retries should be non\-negative\.”
  +  “Find Matches parameters have not been set\.”
  + “A primary key must be specified in Find Matches parameters\.”

  OK to retry: No\.
+ AlreadyExistsException \(400\)
  + “Transform with name \[transformName\] already exists\.”

  OK to retry: No\.
+ IdempotentParameterMismatchException \(400\)
  + “Idempotent create request for transform \[transformName\] had mismatching parameters\.”

  OK to retry: No\.
+ InternalServiceException \(500\)
  + “Dependency failure\.”

  OK to retry: Yes\.
+ ResourceNumberLimitExceededException \(400\)
  + “ML Transforms count \(\[count\]\) has exceeded the limit of \[limit\] transforms\.”

  OK to retry: Yes, once you’ve deleted a transform to make room for this new one\.

## DeleteMLTransformActivity<a name="exceptions-machine-learning-DeleteMLTransformActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]”

  OK to retry: No\.

## GetMLTaskRunActivity<a name="exceptions-machine-learning-GetMLTaskRunActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”
  + “No ML Task Run found for \[taskRunId\]: in account \[accountId\] for transform \[transformName\]\.”

  OK to retry: No\.

## GetMLTaskRunsActivity<a name="exceptions-machine-learning-GetMLTaskRunsActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”
  + “No ML Task Run found for \[taskRunId\]: in account \[accountId\] for transform \[transformName\]\.”

  OK to retry: No\.

## GetMLTransformActivity<a name="exceptions-machine-learning-GetMLTransformActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”

  OK to retry: No\.

## GetMLTransformsActivity<a name="exceptions-machine-learning-GetMLTransformsActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”

  OK to retry: No\.
+ InvalidInputException \(400\)
  + “Account ID can't be blank\.”
  + “Sorting not supported for column \[column\]\.”
  + “\[column\] can't be blank\.”
  + “Internal service failure due to unexpected input\.”

  OK to retry: No\.

## GetSaveLocationForTransformArtifactActivity<a name="exceptions-machine-learning-GetSaveLocationForTransformArtifactActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”

  OK to retry: No\.
+ InvalidInputException \(400\)
  + “Unsupported artifact type \[artifactType\]\.”
  + “Internal service failure due to unexpected input\.”

  OK to retry: No\.

## GetTaskRunArtifactActivity<a name="exceptions-machine-learning-GetTaskRunArtifactActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”
  + “No ML Task Run found for \[taskRunId\]: in account \[accountId\] for transform \[transformName\]\.”

  OK to retry: No\.
+ InvalidInputException \(400\)
  + “File name ‘\[fileName\]’ is invalid for publish\.”
  + “Cannot retrieve artifact for \[taskType\] task type\.”
  + “Cannot retrieve artifact for \[artifactType\]\.”
  + “Internal service failure due to unexpected input\.”

  OK to retry: No\.

## PublishMLTransformModelActivity<a name="exceptions-machine-learning-PublishMLTransformModelActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”
  + “An existing model with version \- \[version\] cannot be found for account id \- \[accountId\] \- and transform id \- \[transformId\]\.”

  OK to retry: No\.
+ InvalidInputException \(400\)
  + “File name ‘\[fileName\]’ is invalid for publish\.”
  + “Illegal leading minus sign on unsigned string \[string\]\.”
  + “Bad digit at end of \[string\]\.”
  +  “String value \[string\] exceeds range of unsigned long\.”
  + “Internal service failure due to unexpected input\.”

  OK to retry: No\.

## PullLatestMLTransformModelActivity<a name="exceptions-machine-learning-PullLatestMLTransformModelActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”

  OK to retry: No\.
+ InvalidInputException \(400\)
  + “Internal service failure due to unexpected input\.”

  OK to retry: No\.
+ ConcurrentModificationException \(400\)
  + “Cannot create model version to train due to racing inserts with mismatching parameters\.”
  + “The ML Transform model for transform id \[transformId\] is stale or being updated by another process; Please retry\.”

  OK to retry: Yes\.

## PutJobMetadataForMLTransformActivity<a name="exceptions-machine-learning-PutJobMetadataForMLTransformActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”
  + “No ML Task Run found for \[taskRunId\]: in account \[accountId\] for transform \[transformName\]\.”

  OK to retry: No\.
+ InvalidInputException \(400\)
  + “Internal service failure due to unexpected input\.”
  + “Unknown job metadata type \[jobType\]\.”
  +  “Must provide a task run ID to update\.”

  OK to retry: No\.

## StartExportLabelsTaskRunActivity<a name="exceptions-machine-learning-StartExportLabelsTaskRunActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”
  + “No labelset exists for transformId \[transformId\] in account id \[accountId\]\.”

  OK to retry: No\.
+ InvalidInputException \(400\)
  + “\[message\]\.”
  + “S3 path provided is not in the same region as transform\. Expecting region \- \[region\], but got \- \[region\]\.”

  OK to retry: No\.

## StartImportLabelsTaskRunActivity<a name="exceptions-machine-learning-StartExportLabelsTaskRunActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”

  OK to retry: No\.
+ InvalidInputException \(400\)
  + “\[message\]\.”
  + “Invalid label file path\.”
  + “Cannot access the label file at \[labelPath\]\. \[message\]\.”
  + “Cannot use IAM role provided in the transform\. Role: \[role\]\.”
  + “Invalid label file of size 0\.”
  + “S3 path provided is not in the same region as transform\. Expecting region \- \[region\], but got \- \[region\]\.”

  OK to retry: No\.
+ ResourceNumberLimitExceededException \(400\)
  + “Label file has exceeded the limit of \[limit\] MB\.”

  OK to retry: No\. Consider breaking your label file into several smaller files\.

## StartMLEvaluationTaskRunActivity<a name="exceptions-machine-learning-StartMLEvaluationTaskRunActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”

  OK to retry: No\.
+ InvalidInputException \(400\)
  + “Exactly one input record table must be provided\.”
  + “Should specify database name\.”
  + “Should specify table name\.”
  + “Find Matches parameters have not been set\.”
  + “A primary key must be specified in Find Matches parameters\.”

  OK to retry: No\.
+ MLTransformNotReadyException \(400\)
  + “This operation can only be applied to a transform that is in a READY state\.”

  OK to retry: No\.
+ InternalServiceException \(500\)
  + “Dependency failure\.”

  OK to retry: Yes\.
+ ConcurrentRunsExceededException \(400\)
  + “ML Task Runs count \[count\] has exceeded the transform limit of \[limit\] task runs\.”
  + “ML Task Runs count \[count\] has exceeded the limit of \[limit\] task runs\.”

  OK to retry: Yes, after waiting for task runs to finish\.

## StartMLLabelingSetGenerationTaskRunActivity<a name="exceptions-machine-learning-StartMLLabelingSetGenerationTaskRunActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”

  OK to retry: No\.
+ InvalidInputException \(400\)
  + “Exactly one input record table must be provided\.”
  + “Should specify database name\.”
  + “Should specify table name\.”
  + “Find Matches parameters have not been set\.”
  + “A primary key must be specified in Find Matches parameters\.”

  OK to retry: No\.
+ InternalServiceException \(500\)
  + “Dependency failure\.”

  OK to retry: Yes\.
+ ConcurrentRunsExceededException \(400\)
  + “ML Task Runs count \[count\] has exceeded the transform limit of \[limit\] task runs\.”

  OK to retry: Yes, after task runs have completed\.

## UpdateMLTransformActivity<a name="exceptions-machine-learning-UpdateMLTransformActivity"></a>

This activity has the following exceptions:
+ EntityNotFoundException \(400\)
  + “Cannot find MLTransform in account \[accountId\] with handle \[transformName\]\.”

  OK to retry: No\.
+ InvalidInputException \(400\)
  + “Another transform with name \[transformName\] already exists\.”
  + “\[message\]\.”
  + “Transform name cannot be blank\.”
  + “Cannot set Max Capacity and Worker Num/Type at the same time\.”
  + “Both WorkerType and NumberOfWorkers should be set\.”
  + “MaxCapacity should be >= \[minMaxCapacity\]\.”
  + “NumberOfWorkers should be >= \[minNumWorkers\]\.”
  + “Max retries should be non\-negative\.”
  + “Internal service failure due to unexpected input\.”
  + “Find Matches parameters have not been set\.”
  + “A primary key must be specified in Find Matches parameters\.”

  OK to retry: No\.
+ AlreadyExistsException \(400\)
  + “Transform with name \[transformName\] already exists\.”

  OK to retry: No\.
+ IdempotentParameterMismatchException \(400\)
  + “Idempotent create request for transform \[transformName\] had mismatching parameters\.”

  OK to retry: No\.