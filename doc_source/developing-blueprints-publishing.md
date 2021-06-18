# Publishing a Blueprint<a name="developing-blueprints-publishing"></a>

After you develop a blueprint, you must upload it to Amazon S3\. You must have write permissions on the Amazon S3 bucket that you use to publish the blueprint\. You must also make sure that the AWS Glue administrator, who will register the blueprint, has read access to the Amazon S3 bucket\. For the suggested AWS Identity and Access Management \(IAM\) permissions policies for personas and roles for AWS Glue blueprints, see [Permissions for Personas and Roles for AWS Glue Blueprints](blueprints-personas-permissions.md)\.

**To publish a blueprint**

1. Create the necessary scripts, resources, and blueprint configuration file\.

1. Add all files to a ZIP archive and upload the ZIP file to Amazon S3\. Use an S3 bucket that is in the same Region as the Region in which users will register and run the blueprint\.

   You can create a ZIP file from the command line using the following command\.

   ```
   zip -r folder.zip folder
   ```

1. Add a bucket policy that grants read permissions to the AWS desired account\. The following is a sample policy\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Principal": {
                   "AWS": "arn:aws:iam::111122223333:root"
               },
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::my-blueprints/*"
           }
       ]
   }
   ```

1. Grant the IAM `s3:GetObject` permission on the Amazon S3 bucket to the AWS Glue administrator or to whoever will be registering blueprints\. For a sample policy to grant to administrators, see [AWS Glue Administrator Permissions for Blueprints](blueprints-personas-permissions.md#bp-persona-admin)\.

After you have completed local testing of your blueprint, you may also want to test a blueprint on AWS Glue\. To test a blueprint on AWS Glue, it must be registered\. You can limit who sees the registered blueprint using IAM authorization, or by using separate testing accounts\.

**See Also:**  
[Registering a Blueprint in AWS Glue](registering-blueprints.md)