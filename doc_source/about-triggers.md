# AWS Glue Triggers<a name="about-triggers"></a>

When *fired*, a trigger can start specified jobs and crawlers\. A trigger fires on demand, based on a schedule, or based on a combination of events\.

**Note**  
Only two crawlers can be activated by a single trigger\. If you want to crawl multiple data stores, use multiple sources for each crawler instead of running multiple crawlers simultaneously\.

A trigger can exist in one of several states\. A trigger is either `CREATED`, `ACTIVATED`, or `DEACTIVATED`\. There are also transitional states, such as `ACTIVATING`\. To temporarily stop a trigger from firing, you can deactivate it\. You can then reactivate it later\.

There are three types of triggers:

**Scheduled**  
A time\-based trigger based on `cron`\.  
You can create a trigger for a set of jobs or crawlers based on a schedule\. You can specify constraints, such as the frequency that the jobs or crawlers run, which days of the week they run, and at what time\. These constraints are based on `cron`\. When you're setting up a schedule for a trigger, consider the features and limitations of cron\. For example, if you choose to run your crawler on day 31 each month, keep in mind that some months don't have 31 days\. For more information about cron, see [Time\-Based Schedules for Jobs and Crawlers](monitor-data-warehouse-schedule.md)\.  

**Conditional**  
A trigger that fires when a previous job or crawler or multiple jobs or crawlers satisfy a list of conditions\.  
 When you create a conditional trigger, you specify a list of jobs and a list of crawlers to watch\. For each watched job or crawler, you specify a status to watch for, such as succeeded, failed, timed out, and so on\. The trigger fires if the watched jobs or crawlers end with the specified statuses\. You can configure the trigger to fire when any or all of the watched events occur\.  
For example, you could configure a trigger T1 to start job J3 when both job J1 and job J2 successfully complete, and another trigger T2 to start job J4 if either job J1 or job J2 fails\.  
The following table lists the job and crawler completion states \(events\) that triggers watch for\.      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/about-triggers.html)

**On\-demand**  
A trigger that fires when you activate it\. On\-demand triggers never enter the `ACTIVATED` or `DEACTIVATED` state\. They always remain in the `CREATED` state\.

So that they are ready to fire as soon as they exist, you can set a flag to activate scheduled and conditional triggers when you create them\.

**Important**  
Jobs or crawlers that run as a result of other jobs or crawlers completing are referred to as *dependent*\. Dependent jobs or crawlers are only started if the job or crawler that completes was started by a trigger\. All jobs or crawlers in a dependency chain must be descendants of a single **scheduled** or **on\-demand** trigger\.

**Passing Job Parameters with Triggers**  
A trigger can pass parameters to the jobs that it starts\. Parameters include job arguments, timeout value, security configuration, and more\. If the trigger starts multiple jobs, the parameters are passed to each job\.

The following are the rules for job arguments passed by a trigger:
+ If the key in the key\-value pair matches a default job argument, the passed argument overrides the default argument\. If the key doesn’t match a default argument, then the argument is passed as an additional argument to the job\.
+ If the key in the key\-value pair matches a non\-overridable argument, the passed argument is ignored\.

For more information, see [Triggers](aws-glue-api-jobs-trigger.md) in the AWS Glue API\.