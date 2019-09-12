# Managing Access Permissions for AWS Glue Resources<a name="access-control-overview"></a>

You can have valid credentials to authenticate your requests, but unless you have the appropriate permissions, you can't create or access an AWS Glue resource such as a table in the AWS Glue Data Catalog\.

Every AWS resource is owned by an AWS account, and permissions to create or access a resource are governed by permissions policies\. An account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\)\. Some services \(such as AWS Glue and Amazon S3\) also support attaching permissions policies to the resources themselves\. 

**Note**  
An *account administrator* \(or administrator user\) is a user who has administrative privileges\. For more information, see [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

When granting permissions, you decide who is getting the permissions, the resources they get permissions for, and the specific actions that you want to allow on those resources\.

**Topics**
+ [Using Permissions Policies to Manage Access to Resources](#access-control-permissions)
+ [AWS Glue Resources and Operations](#access-control-resources)
+ [Understanding Resource Ownership](#access-control-resource-ownership)
+ [Managing Access to Resources](#access-control-manage-access-intro)
+ [Specifying Policy Elements: Actions, Effects, and Principals](#specify-policy-elements)
+ [Specifying Conditions in a Policy](#specifying-conditions-overview)
+ [Identity\-Based Policies \(IAM Policies\) for Access Control](using-identity-based-policies.md)
+ [AWS Glue Resource Policies for Access Control](glue-resource-policies.md)

## Using Permissions Policies to Manage Access to Resources<a name="access-control-permissions"></a>

A *permissions policy* is defined by a JSON object that describes who has access to what\. The syntax of the JSON object is largely defined by AWS Identity and Access Management \(IAM\)\. For more information, see [IAM JSON Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

**Note**  
This section discusses using IAM in the context of AWS Glue, but it does not provide detailed information about the IAM service\. For more information, see [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\.

For a table showing all of the AWS Glue API operations and the resources that they apply to, see [AWS Glue API Permissions: Actions and Resources Reference](api-permissions-reference.md)\.

To learn more about IAM policy syntax and descriptions, see [IAM JSON Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

AWS Glue supports two kinds of policies:
+ [Identity\-Based Policies \(IAM Policies\) for Access Control](using-identity-based-policies.md)
+ [AWS Glue Resource Policies for Access Control](glue-resource-policies.md)

By supporting both identity\-based and resource policies, AWS Glue gives you fine\-grained control over who can access what metadata\.

For more examples, see [AWS Glue Resource\-Based Access Control Policy Examples](glue-policy-examples-resource-policies.md)\.

## AWS Glue Resources and Operations<a name="access-control-resources"></a>

AWS Glue provides a set of operations to work with AWS Glue resources\. For a list of available operations, see [AWS Glue API](aws-glue-api.md)\.

## Understanding Resource Ownership<a name="access-control-resource-ownership"></a>

The AWS account owns the resources that are created in the account, regardless of who created the resources\. Specifically, the resource owner is the AWS account of the [principal entity](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) \(that is, the AWS account root user, an IAM user, or an IAM role\) that authenticates the resource creation request\. The following examples illustrate how this works:
+ If you use the AWS account root user credentials of your AWS account to create a table, your AWS account is the owner of the resource \(in AWS Glue, the resource is a table\)\.
+ If you create an IAM user in your AWS account and grant permissions to create a table to that user, the user can create a table\. However, your AWS account, which the user belongs to, owns the table resource\.
+ If you create an IAM role in your AWS account with permissions to create a table, anyone who can assume the role can create a table\. Your AWS account, to which the user belongs, owns the table resource\. 

## Managing Access to Resources<a name="access-control-manage-access-intro"></a>

A *permissions policy* describes who has access to what\. The following section explains the available options for creating permissions policies\.

**Note**  
This section discusses using IAM in the context of AWS Glue\. It doesn't provide detailed information about the IAM service\. For complete IAM documentation, see [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\. For information about IAM policy syntax and descriptions, see [IAM JSON Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

Policies that are attached to an IAM identity are referred to as *identity\-based* policies \(IAM policies\)\. Policies that are attached to a resource are referred to as *resource\-based* policies\.

**Topics**
+ [Identity\-Based Policies \(IAM Policies\)](#access-control-manage-access-identity-based)
+ [Resource\-Based Policies](#access-control-manage-access-resource-based)

### Identity\-Based Policies \(IAM Policies\)<a name="access-control-manage-access-identity-based"></a>

You can attach policies to IAM identities\. For example, you can do the following:
+ **Attach a permissions policy to a user or a group in your account** – To grant a user permissions to create an AWS Glue resource, such as a table, you can attach a permissions policy to a user or group that the user belongs to\.
+ **Attach a permissions policy to a role \(grant cross\-account permissions\)** – You can attach an identity\-based permissions policy to an IAM role to grant cross\-account permissions\. For example, the administrator in account A can create a role to grant cross\-account permissions to another AWS account \(for example, account B\) or an AWS service as follows:

  1. Account A administrator creates an IAM role and attaches a permissions policy to the role that grants permissions on resources in account A\.

  1. Account A administrator attaches a trust policy to the role identifying account B as the principal who can assume the role\. 

  1. Account B administrator can then delegate permissions to assume the role to any users in account B\. Doing this allows users in account B to create or access resources in account A\. The principal in the trust policy can also be an AWS service principal if you want to grant an AWS service permissions to assume the role\.

  For more information about using IAM to delegate permissions, see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

The following is an example identity\-based policy that grants permissions for one AWS Glue action \(`GetTables`\)\. The wildcard character \(`*`\) in the `Resource` value means that you are granting permission to this action to obtain names and details of all the tables in a database in the Data Catalog\. If the user also has access to other catalogs through a resource policy, it is given access to these resources too\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "GetTables",
            "Effect": "Allow",
            "Action": [
                "glue:GetTables"
            ],
            "Resource": "*"
        }
    ]
}
```

For more information about using identity\-based policies with AWS Glue, see [Identity\-Based Policies \(IAM Policies\) for Access Control](using-identity-based-policies.md)\. For more information about users, groups, roles, and permissions, see [Identities \(Users, Groups, and Roles\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the *IAM User Guide*\. 

### Resource\-Based Policies<a name="access-control-manage-access-resource-based"></a>

Other services, such as Amazon S3, also support resource\-based permissions policies\. For example, you can attach a policy to an S3 bucket to manage access permissions to that bucket\.

## Specifying Policy Elements: Actions, Effects, and Principals<a name="specify-policy-elements"></a>

For each AWS Glue resource, the service defines a set of API operations\. To grant permissions for these API operations, AWS Glue defines a set of actions that you can specify in a policy\. Some API operations can require permissions for more than one action in order to perform the API operation\. For more information about resources and API operations, see [AWS Glue Resources and Operations](#access-control-resources) and AWS Glue [AWS Glue API](aws-glue-api.md)\.

The following are the most basic policy elements:
+ **Resource** – You use an Amazon Resource Name \(ARN\) to identify the resource that the policy applies to\. For more information, see [AWS Glue Resources and Operations](#access-control-resources)\.
+ **Action** – You use action keywords to identify resource operations that you want to allow or deny\. For example, you can use `create` to allow users to create a table\.
+ **Effect** – You specify the effect, either allow or deny, when the user requests the specific action\. If you don't explicitly grant access to \(allow\) a resource, access is implicitly denied\. You can also explicitly deny access to a resource, which you might do to make sure that a user cannot access it, even if a different policy grants access\.
+ **Principal** – In identity\-based policies \(IAM policies\), the user that the policy is attached to is the implicit principal\. For resource\-based policies, you specify the user, account, service, or other entity that you want to receive permissions \(applies to resource\-based policies only\)\. AWS Glue doesn't support resource\-based policies\.

To learn more about IAM policy syntax and descriptions, see [IAM JSON Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

For a table showing all of the AWS Glue API operations and the resources that they apply to, see [AWS Glue API Permissions: Actions and Resources Reference](api-permissions-reference.md)\.

## Specifying Conditions in a Policy<a name="specifying-conditions-overview"></a>

When you grant permissions, you can use the access policy language to specify the conditions when a policy should take effect\. For example, you might want a policy to be applied only after a specific date\. For more information about specifying conditions in a policy language, see [Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

To express conditions, you use predefined condition keys\. There are AWS\-wide condition keys and AWS Glue–specific keys that you can use as appropriate\. For a complete list of AWS\-wide keys, see [Available Keys for Conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.  