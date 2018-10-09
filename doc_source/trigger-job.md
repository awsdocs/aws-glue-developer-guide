# Triggering Jobs in AWS Glue<a name="trigger-job"></a>

You decide what triggers an extract, transform, and load \(ETL\) job to run in AWS Glue\. The triggering condition can be based on a schedule \(as defined by a cron expression\) or on an event\. You can also run a job on demand\.

## Triggering Jobs Based on Schedules or Events<a name="job-trigger"></a>

When you create a trigger for a job based on a schedule, you can specify constraints, such as the frequency the job runs, which days of the week it runs, and at what time\. These constraints are based on *cron*\. When you're setting up a schedule for a trigger, you should consider the features and limitations of cron\. For example, if you choose to run your crawler on day 31 each month, keep in mind that some months don't have 31 days\. For more information about cron, see [Time\-Based Schedules for Jobs and Crawlers](monitor-data-warehouse-schedule.md)\.  

When you create a trigger based on an event, you specify events to watch that cause the trigger to fire, such as when another job succeeded\. For a conditional trigger based on a *job events* trigger, you specify a list of jobs that cause a trigger to fire when any or all jobs satisfy the watched job events\. In turn, when the trigger fires, it starts a run of any dependent jobs\.

## Defining Trigger Types<a name="trigger-defining"></a>

A trigger can be one of the following types: 

**Schedule**  
A time\-based trigger based on cron\.

**Job events \(conditional\)**  
An event\-based trigger that fires when a previous job or multiple jobs satisfy a list of conditions\. You provide a list of job events to watch for when their run state changes to `succeeded`, `failed`, `stopped`, or `timeout`\. This trigger waits to fire until any or all the conditions are satisfied\.   
Dependent jobs are only started if the job which completes was started by a trigger \(not run ad\-hoc\)\.  All jobs in a dependency chain must be descendants of a single **schedule** or **on\-demand** trigger\. 

**On\-demand**  
The trigger fires when you start it\. As jobs complete, any triggers watching for completion are also fired and dependent jobs are started\.

So that they are ready to fire as soon as they exist, you can set a flag to enable \(activate\) **schedule** and **job events \(conditional\)** triggers when they are created\.

 For more information about defining triggers using the AWS Glue console, see [Working with Triggers on the AWS Glue Console](console-triggers.md)\. 