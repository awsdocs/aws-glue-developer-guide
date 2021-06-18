# Testing a Blueprint<a name="developing-blueprints-testing"></a>

While you develop your code, you should perform local testing to verify that the workflow layout is correct\.

Local testing doesn't generate AWS Glue jobs, crawlers, or triggers\. Instead, you run the layout script locally and use the `to_json()` and `validate()` methods to print objects and find errors\. These methods are available in all three classes defined in the libraries\. 

There are two ways to handle the `user_params` and `system_params` arguments that AWS Glue passes to your layout function\. Your test\-bench code can create a dictionary of sample blueprint parameter values and pass that to the layout function as the `user_params` argument\. Or, you can remove the references to `user_params` and replace them with hardcoded strings\.

If your code makes use of the `region` and `accountId` properties in the `system_params` argument, you can pass in your own dictionary for `system_params`\.

**To test a blueprint**

1. Start a Python interpreter in a directory with the libraries, or load the blueprint files and the supplied libraries into your preferred integrated development environment \(IDE\)\.

1. Ensure that your code imports the supplied libraries\.

1. Add code to your layout function to call `validate()` or `to_json()` on any entity or on the `Workflow` object\. For example, if your code creates a `Crawler` object named `mycrawler`, you can call `validate()` as follows\.

   ```
   mycrawler.validate()
   ```

   You can print `mycrawler` as follows:

   ```
   print(mycrawler.to_json())
   ```

   If you call `to_json` on an object, there is no need to also call `validate()`, because` to_json()` calls `validate()`\. 

   It is most useful to call these methods on the workflow object\. Assuming that your script names the workflow object `my_workflow`, validate and print the workflow object as follows\.

   ```
   print(my_workflow.to_json())
   ```

   For more information about `to_json()` and `validate()`, see [Class Methods](developing-blueprints-code-classes.md#developing-blueprints-code-methods)\.

   You can also import `pprint` and pretty\-print the workflow object, as shown in the example later in this section\.

1. Run the code, fix errors, and finally remove any calls to `validate()` or `to_json()`\.

**Example**  
The following example shows how to construct a dictionary of sample blueprint parameters and pass it in as the `user_params` argument to layout function `generate_compaction_workflow`\. It also shows how to pretty\-print the generated workflow object\.  

```
from pprint import pprint
from awsglue.blueprint.workflow import *
from awsglue.blueprint.job import *
from awsglue.blueprint.crawler import *
 
USER_PARAMS = {"WorkflowName": "compaction_workflow",
               "ScriptLocation": "s3://awsexamplebucket1/scripts/threaded-compaction.py",
               "PassRole": "arn:aws:iam::111122223333:role/GlueRole-ETL",
               "DatabaseName": "cloudtrial",
               "TableName": "ct_cloudtrail",
               "CoalesceFactor": 4,
               "MaxThreadWorkers": 200}
 
 
def generate_compaction_workflow(user_params: dict, system_params: dict) -> Workflow:
    compaction_job = Job(Name=f"{user_params['WorkflowName']}_etl_job",
                         Command={"Name": "glueetl",
                                  "ScriptLocation": user_params['ScriptLocation'],
                                  "PythonVersion": "3"},
                         Role="arn:aws:iam::111122223333:role/AWSGlueServiceRoleDefault",
                         DefaultArguments={"DatabaseName": user_params['DatabaseName'],
                                           "TableName": user_params['TableName'],
                                           "CoalesceFactor": user_params['CoalesceFactor'],
                                           "max_thread_workers": user_params['MaxThreadWorkers']})
 
    catalog_target = {"CatalogTargets": [{"DatabaseName": user_params['DatabaseName'], "Tables": [user_params['TableName']]}]}
 
    compacted_files_crawler = Crawler(Name=f"{user_params['WorkflowName']}_post_crawl",
                                      Targets = catalog_target,
                                      Role=user_params['PassRole'],
                                      DependsOn={compaction_job: "SUCCEEDED"},
                                      WaitForDependencies="AND",
                                      SchemaChangePolicy={"DeleteBehavior": "LOG"})
 
    compaction_workflow = Workflow(Name=user_params['WorkflowName'],
                                   Entities=Entities(Jobs=[compaction_job],
                                                     Crawlers=[compacted_files_crawler]))
    return compaction_workflow
 
generated = generate_compaction_workflow(user_params=USER_PARAMS, system_params={})
gen_dict = generated.to_json()
 
pprint(gen_dict)
```