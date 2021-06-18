# Adding Triggers<a name="console-triggers"></a>

You can add a trigger using the AWS Glue console, the AWS Command Line Interface \(AWS CLI\), or the AWS Glue API\.

**Note**  
Currently, the AWS Glue console supports only jobs, not crawlers, when working with triggers\. You can use the AWS CLI or AWS Glue API to configure triggers with both jobs and crawlers\.

**To add a trigger \(console\)**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\. 

1. In the navigation pane, under **ETL**, choose **Triggers**\. Then choose **Add trigger**\.

1. Provide the following properties:  
**Name**  
Give your trigger a unique name\.  
**Trigger type**  
Specify one of the following:  
   + **Schedule:** The trigger fires at a specific frequency and time\.
   + **Job events:** A conditional trigger\. The trigger fires when any or all jobs in the list match their designated statuses\. For the trigger to fire, the watched jobs must have been started by triggers\. For any job you choose, you can only watch one job event \(completion status\)\.
   + **On\-demand:** The trigger fires when it is activated\.

1. Complete the trigger wizard\. On the **Review** page, you can activate **Schedule** and **Job events** \(conditional\) triggers immediately by selecting **Enable trigger on creation**\.

**To add a trigger \(AWS CLI\)**
+ Enter a command similar to the following\.

  ```
  aws glue create-trigger --name MyTrigger --type SCHEDULED --schedule  "cron(0 12 * * ? *)" --actions CrawlerName=MyCrawler --start-on-creation  
  ```

  This command creates a schedule trigger named `MyTrigger`, which runs every day at 12:00pm UTC and starts a crawler named `MyCrawler`\. The trigger is created in the activated state\.

For more information, see [AWS Glue Triggers](about-triggers.md)\.