# Tutorial: Use a REPL Shell with Your Development Endpoint<a name="dev-endpoint-tutorial-repl"></a>

 In AWS Glue, you can create a development endpoint and then invoke a REPL \(Read–Evaluate–Print Loop\) shell to run PySpark code incrementally so that you can interactively debug your ETL scripts before deploying them\.

The tutorial assumes that you have already taken the steps outlined in [Tutorial Prerequisites](dev-endpoint-tutorial-prerequisites.md)\.

1. In the AWS Glue console, choose **Dev endpoints** to navigate to the development endpoints list\. Choose the name of a development endpoint to open its details page\.

1. Copy the SSH command labeled **SSH to Python REPL**, and paste it into a text editor\. This field is only shown if the development endpoint contains a public SSH key\. Replace the `<private-key.pem>` text with the path to the private\-key `.pem` file that corresponds to the public key that you used to create the development endpoint\. Use forward slashes rather than backslashes as delimiters in the path\.

1. On your local computer, open a terminal window that can run SSH commands, and paste in the edited SSH command\. Run the command\. The output will look like this:

   ```
   download: s3://aws-glue-jes-prod-us-east-1-assets/etl/jars/glue-assembly.jar to ../../usr/share/aws/glue/etl/jars/glue-assembly.jar
   download: s3://aws-glue-jes-prod-us-east-1-assets/etl/python/PyGlue.zip to ../../usr/share/aws/glue/etl/python/PyGlue.zip
   Python 2.7.12 (default, Sep  1 2016, 22:14:00)
   [GCC 4.8.3 20140911 (Red Hat 4.8.3-9)] on linux2
   Type "help", "copyright", "credits" or "license" for more information.
   Setting default log level to "WARN".
   To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
   SLF4J: Class path contains multiple SLF4J bindings.
   SLF4J: Found binding in [jar:file:/usr/share/aws/glue/etl/jars/glue-assembly.jar!/org/slf4j/impl/StaticLoggerBinder.class]
   SLF4J: Found binding in [jar:file:/usr/lib/spark/jars/slf4j-log4j12-1.7.16.jar!/org/slf4j/impl/StaticLoggerBinder.class]
   SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
   SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
   Welcome to
         ____              __
        / __/__  ___ _____/ /__
       _\ \/ _ \/ _ `/ __/  '_/
      /__ / .__/\_,_/_/ /_/\_\   version 2.1.0
         /_/
   
   Using Python version 2.7.12 (default, Sep  1 2016 22:14:00)
   SparkSession available as 'spark'.
   >>>
   ```

1. Test that the REPL shell is working correctly by typing the statement, `print spark.version`\. As long as that displays the Spark version, your REPL is now ready to use\.

1. Now you can try executing the following simple script, line by line, in the shell:

   ```
   import sys
   from pyspark.context import SparkContext
   from awsglue.context import GlueContext
   from awsglue.transforms import *
   glueContext = GlueContext(SparkContext.getOrCreate())
   persons_DyF = glueContext.create_dynamic_frame.from_catalog(database="legislators", table_name="persons_json")
   print "Count:  ", persons_DyF.count()
   persons_DyF.printSchema()
   ```