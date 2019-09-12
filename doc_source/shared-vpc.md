# Shared Amazon VPCs<a name="shared-vpc"></a>

AWS Glue supports shared Amazon virtual private clouds \(VPCs\)\. Amazon VPC sharing allows multiple AWS accounts to create their application resources, such as Amazon EC2 instances and Amazon Relational Database Service \(RDS\) databases, into shared, centrally\-managed Amazon VPCs\. In this model, the account that owns the VPC \(owner\) shares one or more subnets with other accounts \(participants\) that belong to the same organization from AWS Organizations\. After a subnet is shared, the participants can view, create, modify, and delete their application resources in the subnets that are shared with them\.

In AWS Glue, to create a connection with a shared subnet, you must create a security group within your account and attach the security group to the shared subnet\.

For more information, see these topics:
+ [Working with Shared VPCs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-sharing.html) in the *Amazon VPC User Guide*
+ [What Is AWS Organizations? ](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html) in the *AWS Organizations User Guide*