# Setting up IAM Permissions for AWS Glue<a name="getting-started-access"></a>

You use AWS Identity and Access Management \(IAM\) to define policies and roles that are needed to access resources used by AWS Glue\. The following steps lead you through the basic permissions that you need to set up your environment\. Depending on your business needs, you might have to add or reduce access to your resources\.

1. [Create an IAM Policy for the AWS Glue Service](create-service-policy.md): Create a service policy that allows access to AWS Glue resources\.

1. [Create an IAM Role for AWS Glue](create-an-iam-role.md): Create an IAM role, and attach the AWS Glue service policy and a policy for your Amazon Simple Storage Service \(Amazon S3\) resources that are used by AWS Glue\.

1. [Attach a Policy to IAM Users That Access AWS Glue](attach-policy-iam-user.md): Attach policies to any IAM user that signs in to the AWS Glue console\.

1. [Create an IAM Policy for Notebooks](create-notebook-policy.md): Create a notebook server policy to use in the creation of notebook servers on development endpoints\.

1. [Create an IAM Role for Notebooks](create-an-iam-role-notebook.md): Create an IAM role and attach the notebook server policy\.

1. [Create an IAM Policy for Amazon SageMaker Notebooks](create-sagemaker-notebook-policy.md): Create an IAM policy to use when creating Amazon SageMaker notebooks on development endpoints\.

1. [Create an IAM Role for Amazon SageMaker Notebooks](create-an-iam-role-sagemaker-notebook.md): Create an IAM role and attach the policy to grant permissions when creating Amazon SageMaker notebooks on development endpoints\.