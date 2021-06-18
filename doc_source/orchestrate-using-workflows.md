# Performing Complex ETL Activities Using Blueprints and Workflows in AWS Glue<a name="orchestrate-using-workflows"></a>


|  | 
| --- |
| The AWS Glue blueprints feature is in preview release for AWS Glue and is subject to change\. | 

Some of your organization's complex extract, transform, and load \(ETL\) processes might best be implemented by using multiple, dependent AWS Glue jobs and crawlers\. Using AWS Glue *workflows*, you can design a complex multi\-job, multi\-crawler ETL process that AWS Glue can run and track as single entity\. After you create a workflow and specify the jobs, crawlers, and triggers in the workflow, you can run the workflow on demand or on a schedule\.

Your organization might have a set of similar ETL use cases that could benefit from being able to parameterize a single workflow to handle them all\. To address this need, AWS Glue enables you to define *blueprints*, which you can use to generate workflows\. A blueprint accepts parameters, so that from a single blueprint, a data analyst can create different workflows to handle similar ETL use cases\. After you create a blueprint, you can reuse it for different departments, teams, and projects\.

**Topics**
+ [Overview of Workflows in AWS Glue](workflows_overview.md)
+ [Overview of Blueprints in AWS Glue](blueprints-overview.md)
+ [Developing Blueprints in AWS Glue](developing-blueprints.md)
+ [Registering a Blueprint in AWS Glue](registering-blueprints.md)
+ [Viewing Blueprints in AWS Glue](viewing_blueprints.md)
+ [Updating a Blueprint in AWS Glue](updating_blueprints.md)
+ [Creating a Workflow from a Blueprint in AWS Glue](creating_workflow_blueprint.md)
+ [Viewing Blueprint Runs in AWS Glue](viewing_blueprint_runs.md)
+ [Creating and Building Out a Workflow Manually in AWS Glue](creating_running_workflows.md)
+ [Running and Monitoring a Workflow in AWS Glue](running_monitoring_workflow.md)
+ [Stopping a Workflow Run](workflow-stopping.md)
+ [Repairing and Resuming a Workflow Run](resuming-workflow.md)
+ [Getting and Setting Workflow Run Properties in AWS Glue](workflow-run-properties-code.md)
+ [Querying Workflows Using the AWS Glue API](workflows_api_concepts.md)
+ [Blueprint and Workflow Restrictions in AWS Glue](blueprint_workflow_restrictions.md)
+ [Permissions for Personas and Roles for AWS Glue Blueprints](blueprints-personas-permissions.md)