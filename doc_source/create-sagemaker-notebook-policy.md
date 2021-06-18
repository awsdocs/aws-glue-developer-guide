# Step 6: Create an IAM Policy for SageMaker Notebooks<a name="create-sagemaker-notebook-policy"></a>

If you plan to use SageMaker notebooks with development endpoints, you must specify permissions when you create the notebook\. You provide those permissions by using AWS Identity and Access Management \(IAM\)\.

**To create an IAM policy for SageMaker notebooks**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the left navigation pane, choose **Policies**\.

1. Choose **Create Policy**\.

1. On the **Create Policy** page, navigate to a tab to edit the JSON\. Create a policy document with the following JSON statements\. Edit *bucket\-name*, *region\-code*, and *account\-id* for your environment\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Action": [
                   "s3:ListBucket"
               ],
               "Effect": "Allow",
               "Resource": [
                   "arn:aws:s3:::bucket-name"
               ]
           },
           {
               "Action": [
                   "s3:GetObject"
               ],
               "Effect": "Allow",
               "Resource": [
                   "arn:aws:s3:::bucket-name*"
               ]
           },
           {
               "Action": [
                   "logs:CreateLogStream",
                   "logs:DescribeLogStreams",
                   "logs:PutLogEvents",
                   "logs:CreateLogGroup"
               ],
               "Effect": "Allow",
               "Resource": [
                   "arn:aws:logs:region-code:account-id:log-group:/aws/sagemaker/*",
                   "arn:aws:logs:region-code:account-id:log-group:/aws/sagemaker/*:log-stream:aws-glue-*"
               ]
           },
           {
               "Action": [
                   "glue:UpdateDevEndpoint",
                   "glue:GetDevEndpoint",
                   "glue:GetDevEndpoints"
               ],
               "Effect": "Allow",
               "Resource": [
                   "arn:aws:glue:region-code:account-id:devEndpoint/*"
               ]
           },
           {
               "Action": [
                   "sagemaker:ListTags"
               ],
               "Effect": "Allow",
               "Resource": [
                   "arn:aws:sagemaker:region-code:account-id:notebook-instance/*"
               ]
            }
       ]
   }
   ```

   Then choose **Review policy**\. 

   The following table describes the permissions granted by this policy\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/create-sagemaker-notebook-policy.html)

1. On the **Review Policy** screen, enter your **Policy Name**, for example **AWSGlueSageMakerNotebook**\. Enter an optional description, and when you're satisfied with the policy, choose **Create policy**\.