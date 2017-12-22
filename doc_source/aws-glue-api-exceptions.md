# Exceptions<a name="aws-glue-api-exceptions"></a>

## AccessDeniedException Structure<a name="aws-glue-api-exceptions-AccessDeniedException"></a>

Access to a resource was denied\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## AlreadyExistsException Structure<a name="aws-glue-api-exceptions-AlreadyExistsException"></a>

A resource to be created or added already exists\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## ConcurrentModificationException Structure<a name="aws-glue-api-exceptions-ConcurrentModificationException"></a>

Two processes are trying to modify a resource simultaneously\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## ConcurrentRunsExceededException Structure<a name="aws-glue-api-exceptions-ConcurrentRunsExceededException"></a>

Too many jobs are being run concurrently\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## CrawlerNotRunningException Structure<a name="aws-glue-api-exceptions-CrawlerNotRunningException"></a>

The specified crawler is not running\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## CrawlerRunningException Structure<a name="aws-glue-api-exceptions-CrawlerRunningException"></a>

The operation cannot be performed because the crawler is already running\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## CrawlerStoppingException Structure<a name="aws-glue-api-exceptions-CrawlerStoppingException"></a>

The specified crawler is stopping\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## EntityNotFoundException Structure<a name="aws-glue-api-exceptions-EntityNotFoundException"></a>

A specified entity does not exist

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## IdempotentParameterMismatchException Structure<a name="aws-glue-api-exceptions-IdempotentParameterMismatchException"></a>

The same unique identifier was associated with two different records\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## InternalServiceException Structure<a name="aws-glue-api-exceptions-InternalServiceException"></a>

An internal service error occurred\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## InvalidExecutionEngineException Structure<a name="aws-glue-api-exceptions-InvalidExecutionEngineException"></a>

An unknown or invalid execution engine was specified\.

**Fields**

+ `message` – String\.

  A message describing the problem\.

## InvalidInputException Structure<a name="aws-glue-api-exceptions-InvalidInputException"></a>

The input provided was not valid\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## InvalidTaskStatusTransitionException Structure<a name="aws-glue-api-exceptions-InvalidTaskStatusTransitionException"></a>

Proper transition from one task to the next failed\.

**Fields**

+ `message` – String\.

  A message describing the problem\.

## JobDefinitionErrorException Structure<a name="aws-glue-api-exceptions-JobDefinitionErrorException"></a>

A job definition is not valid\.

**Fields**

+ `message` – String\.

  A message describing the problem\.

## JobRunInTerminalStateException Structure<a name="aws-glue-api-exceptions-JobRunInTerminalStateException"></a>

The terminal state of a job run signals a failure\.

**Fields**

+ `message` – String\.

  A message describing the problem\.

## JobRunInvalidStateTransitionException Structure<a name="aws-glue-api-exceptions-JobRunInvalidStateTransitionException"></a>

A job run encountered an invalid transition from source state to target state\.

**Fields**

+ `jobRunId` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The Id of the job run in question\.

+ `message` – String\.

  A message describing the problem\.

+ `sourceState` – String \(valid values: `STARTING` | `RUNNING` | `STOPPING` | `STOPPED` | `SUCCEEDED` | `FAILED`\)\.

  The source state\.

+ `targetState` – String \(valid values: `STARTING` | `RUNNING` | `STOPPING` | `STOPPED` | `SUCCEEDED` | `FAILED`\)\.

  The target state\.

## JobRunNotInTerminalStateException Structure<a name="aws-glue-api-exceptions-JobRunNotInTerminalStateException"></a>

A job run is not in a terminal state\.

**Fields**

+ `message` – String\.

  A message describing the problem\.

## LateRunnerException Structure<a name="aws-glue-api-exceptions-LateRunnerException"></a>

A job runner is late\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## NoScheduleException Structure<a name="aws-glue-api-exceptions-NoScheduleException"></a>

There is no applicable schedule\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## OperationTimeoutException Structure<a name="aws-glue-api-exceptions-OperationTimeoutException"></a>

The operation timed out\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## ResourceNumberLimitExceededException Structure<a name="aws-glue-api-exceptions-ResourceNumberLimitExceededException"></a>

A resource numerical limit was exceeded\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## SchedulerNotRunningException Structure<a name="aws-glue-api-exceptions-SchedulerNotRunningException"></a>

The specified scheduler is not running\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## SchedulerRunningException Structure<a name="aws-glue-api-exceptions-SchedulerRunningException"></a>

The specified scheduler is already running\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## SchedulerTransitioningException Structure<a name="aws-glue-api-exceptions-SchedulerTransitioningException"></a>

The specified scheduler is transitioning\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## UnrecognizedRunnerException Structure<a name="aws-glue-api-exceptions-UnrecognizedRunnerException"></a>

The job runner was not recognized\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## ValidationException Structure<a name="aws-glue-api-exceptions-ValidationException"></a>

A value could not be validated\.

**Fields**

+ `Message` – String\.

  A message describing the problem\.

## VersionMismatchException Structure<a name="aws-glue-api-exceptions-VersionMismatchException"></a>

There was a version conflict\.

**Fields**

+ `message` – String\.