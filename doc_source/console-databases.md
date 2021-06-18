# Working with Databases on the AWS Glue Console<a name="console-databases"></a>

A database in the AWS Glue Data Catalog is a container that holds tables\. You use databases to organize your tables into separate categories\. Databases are created when you run a crawler or add a table manually\. The database list in the AWS Glue console displays descriptions for all your databases\.

To view the list of databases, sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\. Choose **Databases**, and then choose a database name in the list to view the details\.

From the **Databases** tab in the AWS Glue console, you can add, edit, and delete databases:
+ To create a new database, choose **Add database** and provide a name and description\. For compatibility with other metadata stores, such as Apache Hive, the name is folded to lowercase characters\.
**Note**  
If you plan to access the database from Amazon Athena, then provide a name with only alphanumeric and underscore characters\. For more information, see [Athena names](https://docs.aws.amazon.com/athena/latest/ug/tables-databases-columns-names.html#ate-table-database-and-column-names-allow-only-underscore-special-characters)\.
+ To edit the description for a database, select the check box next to the database name and choose **Action**, **Edit database**\.
+ To delete a database, select the check box next to the database name and choose **Action**, **Delete database**\.
+ To display the list of tables contained in the database, select the check box next to the database name and choose **View tables**\.

To change the database that a crawler writes to, you must change the crawler definition\. For more information, see [Defining Crawlers](add-crawler.md)\.