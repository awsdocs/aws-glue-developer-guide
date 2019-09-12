# Key Management<a name="key-management"></a>

You can use AWS Identity and Access Management \(IAM\) with AWS Glue to define users, AWS resources, groups, roles and fine\-grained policies regarding access, denial, and more\.

You can define the access to the metadata using both resource\-based and identity\-based policies, depending on your organization’s needs\. Resource\-based policies list the principals that are allowed or denied access to your resources, allowing you to set up policies such as cross\-account access\. Identity policies are specifically attached to users, groups, and roles within IAM\. 

For a step\-by\-step example, see [Restrict access to your AWS Glue Data Catalog with resource\-level IAM permissions and resource\-based policies](https://aws.amazon.com/blogs/big-data/restrict-access-to-your-aws-glue-data-catalog-with-resource-level-iam-permissions-and-resource-based-policies/) on the AWS Big Data Blog\.

The fine\-grained access portion of the policy is defined within the `Resource` clause\. This portion defines both the AWS Glue Data Catalog object that the action can be performed on, and what resulting objects get returned by that operation\. 

A *development endpoint* is an environment that you can use to develop and test your AWS Glue scripts\. You can add, delete, or rotate the SSH key of a development endpoint\. 

As of September 4, 2018, AWS KMS \(*bring your own key* and *server\-side encryption*\) for AWS Glue ETL and the AWS Glue Data Catalog is supported\.