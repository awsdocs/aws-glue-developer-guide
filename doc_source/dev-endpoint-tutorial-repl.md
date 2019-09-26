# Tutorial: Use a REPL Shell with Your Development Endpoint<a name="dev-endpoint-tutorial-repl"></a>

 In AWS Glue, you can create a development endpoint and then invoke a REPL \(Read–Evaluate–Print Loop\) shell to run PySpark code incrementally so that you can interactively debug your ETL scripts before deploying them\.

The tutorial assumes that you have already taken the steps outlined in [Tutorial Prerequisites](dev-endpoint-tutorial-prerequisites.md)\.

1. In the AWS Glue console, choose **Dev endpoints** to navigate to the development endpoints list\. Choose the name of a development endpoint to open its details page\.

1. Copy the SSH command labeled **SSH to Python REPL**, and paste it into a text editor\. This field is only shown if the development endpoint contains a public SSH key\. Replace the `<private-key.pem>` text with the path to the private\-key `.pem` file that corresponds to the public key that you used to create the development endpoint\. Use forward slashes rather than backslashes as delimiters in the path\.

1. On your local computer, open a terminal window that can run SSH commands, and paste in the edited SSH command\. Run the command\.

   Assuming that you accepted the default AWS Glue version 1\.0 with Python 3 for the development endpoint, the output will look like this:

   ```
   Python 3.6.8 (default, Aug  2 2019, 17:42:44)
   [GCC 4.8.5 20150623 (Red Hat 4.8.5-28)] on linux
   Type "help", "copyright", "credits" or "license" for more information.
   SLF4J: Class path contains multiple SLF4J bindings.
   SLF4J: Found binding in [jar:file:/usr/share/aws/glue/etl/jars/glue-assembly.jar!/org/slf4j/impl/StaticLoggerBinder.class]
   SLF4J: Found binding in [jar:file:/usr/lib/spark/jars/slf4j-log4j12-1.7.16.jar!/org/slf4j/impl/StaticLoggerBinder.class]
   SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
   SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
   Setting default log level to "WARN".
   To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
   2019-09-23 22:12:23,071 WARN  [Thread-5] yarn.Client (Logging.scala:logWarning(66)) - Neither spark.yarn.jars nor spark.yarn.archive is set, falling back to uploading libraries under SPARK_HOME.
   2019-09-23 22:12:26,562 WARN  [Thread-5] yarn.Client (Logging.scala:logWarning(66)) - Same name resource file:/usr/lib/spark/python/lib/pyspark.zip added multiple times to distributed cache
   2019-09-23 22:12:26,580 WARN  [Thread-5] yarn.Client (Logging.scala:logWarning(66)) - Same path resource file:///usr/share/aws/glue/etl/python/PyGlue.zip added multiple times to distributed cache.
   2019-09-23 22:12:26,581 WARN  [Thread-5] yarn.Client (Logging.scala:logWarning(66)) - Same path resource file:///usr/lib/spark/python/lib/py4j-src.zip added multiple times to distributed cache.
   2019-09-23 22:12:26,581 WARN  [Thread-5] yarn.Client (Logging.scala:logWarning(66)) - Same path resource file:///usr/share/aws/glue/libs/pyspark.zip added multiple times to distributed cache.
   Welcome to
         ____              __
        / __/__  ___ _____/ /__
       _\ \/ _ \/ _ `/ __/  '_/
      /__ / .__/\_,_/_/ /_/\_\   version 2.4.3
         /_/
   
   Using Python version 3.6.8 (default, Aug  2 2019 17:42:44)
   SparkSession available as 'spark'.
   >>>
   ```

1. Test that the REPL shell is working correctly by typing the statement, `print(spark.version)`\. As long as that displays the Spark version, your REPL is now ready to use\.

1. Now you can try executing the following simple script, line by line, in the shell:

   ```
   import sys
   from pyspark.context import SparkContext
   from awsglue.context import GlueContext
   from awsglue.transforms import *
   glueContext = GlueContext(SparkContext.getOrCreate())
   persons_DyF = glueContext.create_dynamic_frame.from_catalog(database="legislators", table_name="persons_json")
   print ("Count:  ", persons_DyF.count())
   persons_DyF.printSchema()
   ```