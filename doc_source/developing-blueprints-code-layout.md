# Creating the Blueprint Layout Script<a name="developing-blueprints-code-layout"></a>

The blueprint layout script must include a function that generates the entities in your workflow\. You can name this function whatever you like\. AWS Glue uses the configuration file to determine the fully qualified name of the function\.

Your layout function does the following:
+ \(Optional\) Instantiates the `Job` class to create `Job` objects, and passes arguments such as `Command` and `Role`\. These are job properties that you would specify if you were creating the job using the AWS Glue console or API\.
+ \(Optional\) Instantiates the `Crawler` class to create `Crawler` objects, and passes name, role, and target arguments\.
+ To indicate dependencies between the objects \(workflow entities\), passes the `DependsOn` and `WaitForDependencies` additional arguments to `Job()` and `Crawler()`\. These arguments are explained later in this section\.
+ Instantiates the `Workflow` class to create the workflow object that is returned to AWS Glue, passing a `Name` argument, an `Entities` argument, and an optional `OnSchedule` argument\. The `Entities` argument specifies all of the jobs and crawlers to include in the workflow\. To see how to construct an `Entities` object, see the sample project later in this section\.
+ Returns the `Workflow` object\.

For definitions of the `Job`, `Crawler`, and `Workflow` classes, see [AWS Glue Blueprint Classes Reference](developing-blueprints-code-classes.md)\.

The layout function must accept the following input arguments\.


| Argument | Description | 
| --- | --- | 
| user\_params | Python dictionary of blueprint parameter names and values\. For more information, see [Specifying Blueprint Parameters](developing-blueprints-code-parameters.md)\. | 
| system\_params | Python dictionary containing two properties: region and accountId\. | 

Here is a sample layout generator script in a file named `Layout.py`:

```
import argparse
import sys
import os
import json
from awsglue.blueprint.workflow import *
from awsglue.blueprint.job import *
from awsglue.blueprint.crawler import *


def generate_layout(user_params, system_params):

    etl_job = Job(Name="{}_etl_job".format(user_params['WorkflowName']),
                  Command={
                      "Name": "glueetl",
                      "ScriptLocation": user_params['ScriptLocation'],
                      "PythonVersion": "2"
                  },
                  Role=user_params['PassRole'])
    post_process_job = Job(Name="{}_post_process".format(user_params['WorkflowName']),
                            Command={
                                "Name": "pythonshell",
                                "ScriptLocation": user_params['ScriptLocation'],
                                "PythonVersion": "2"
                            },
                            Role=user_params['PassRole'],
                            DependsOn={
                                etl_job: "SUCCEEDED"
                            },
                            WaitForDependencies="AND")
    sample_workflow = Workflow(Name=user_params['WorkflowName'],
                            Entities=Entities(Jobs=[etl_job, post_process_job]))
    return sample_workflow
```

The sample script imports the required blueprint libraries and includes a `generate_layout` function that generates a workflow with two jobs\. This is a very simple script\. A more complex script could employ additional logic and parameters to generate a workflow with many jobs and crawlers, or even a variable number of jobs and crawlers\.

## Using the DependsOn Argument<a name="developing-blueprints-code-layout-depends-on"></a>

The `DependsOn` argument is a dictionary representation of a dependency that this entity has on other entities within the workflow\. It has the following form\. 

```
DependsOn = {dependency1 : state, dependency2 : state, ...}
```

The keys in this dictionary represent the object reference, not the name, of the entity, while the values are strings that correspond to the state to watch for\. AWS Glue infers the proper triggers\. For the valid states, see [Condition Structure](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-jobs-trigger.html#aws-glue-api-jobs-trigger-Condition)\.

For example, a job might depend on the successful completion of a crawler\. If you define a crawler object named `crawler2` as follows:

```
crawler2 = Crawler(Name="my_crawler", ...)
```

Then an object depending on `crawler2` would include a constructor argument such as: 

```
DependsOn = {crawler2 : "SUCCEEDED"}
```

For example:

```
job1 = Job(Name="Job1", ..., DependsOn = {crawler2 : "SUCCEEDED", ...})
```

If `DependsOn` is omitted for an entity, that entity depends on the workflow start trigger\.

## Using the WaitForDependencies Argument<a name="developing-blueprints-code-layout-wait-for-dependencies"></a>

The `WaitForDependencies` argument defines whether a job or crawler entity should wait until *all* entities on which it depends complete or until *any* completes\.

The allowable values are "`AND`" or "`ANY`"\.

## Using the OnSchedule Argument<a name="developing-blueprints-code-layout-on-schedule"></a>

The `OnSchedule` argument for the `Workflow` class constructor is a `cron` expression that defines the starting trigger definition for a workflow\.

If this argument is specified, AWS Glue creates a schedule trigger with the corresponding schedule\. If it isn't specified, the starting trigger for the workflow is an on\-demand trigger\.