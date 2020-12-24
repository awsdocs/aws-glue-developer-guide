# Testing an AWS Glue Connection<a name="console-test-connections"></a>

As a best practice, before you use an AWS Glue connection in an extract, transform, and load \(ETL\) job, use the AWS Glue console to test the connection\. AWS Glue uses the parameters in your connection to confirm that it can access your data store, and reports any errors\. For information about AWS Glue connections, see [AWS Glue Connections](connection-using.md)\.

**To test an AWS Glue connection**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, under **Data catalog**, choose **Connections**\.

1. Select the check box next to the desired connection, and then choose **Test connection**\.

1. In the **Test connection** dialog box, select a role or choose **Create IAM role** to go to the AWS Identity and Access Management \(IAM\) console to create a new role\. The role must have permissions on the data store\.

1. Choose **Test connection**\.

   The test begins and can take several minutes to complete\. If the test fails, choose **logs** in the banner at the top of the screen to view the Amazon CloudWatch Logs\.

   You must have the required IAM permissions to view the logs\. For more information, see [AWS Managed \(Predefined\) Policies for CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/iam-identity-based-access-control-cwl.html#managed-policies-cwl) in the *Amazon CloudWatch Logs User Guide*\.