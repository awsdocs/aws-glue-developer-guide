# Defining a Database in Your Data Catalog<a name="define-database"></a>

Databases are used to organize metadata tables in the AWS Glue\. When you define a table in the AWS Glue Data Catalog, you add it to a database\. A table can be in only one database\.

Your database can contain tables that define data from many different data stores\. This data can include objects in Amazon Simple Storage Service \(Amazon S3\) and relational tables in Amazon Relational Database Service\.

**Note**  
When you delete a database from the AWS Glue Data Catalog, all the tables in the database are also deleted\.

 For more information about defining a database using the AWS Glue console, see [Working with Databases on the AWS Glue Console](console-databases.md)\. 

## Database Resource Links<a name="databases-resource-links"></a>

The Data Catalog can also contain *resource links* to databases\. A database resource link is a link to a local or shared database\. Currently, you can create resource links only in AWS Lake Formation\. After you create a resource link to a database, you can use the resource link name wherever you would use the database name\. Along with databases that you own or that are shared with you, database resource links are returned by `glue:GetDatabases()` and appear as entries on the **Databases** page of the AWS Glue console\.

The Data Catalog can also contain table resource links\.

For more information about resource links, see [Creating Resource Links](https://docs.aws.amazon.com/lake-formation/latest/dg/creating-resource-links.html) in the *AWS Lake Formation Developer Guide*\.