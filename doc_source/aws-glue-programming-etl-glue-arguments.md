# Special Parameters Used by AWS Glue<a name="aws-glue-programming-etl-glue-arguments"></a>

There are a number of argument names that are recognized and used by AWS Glue, that you can use to set up the script environment for your Jobs and JobRuns:
+ `--job-language`  —  The script programming language\. This must be either `scala` or `python`\. If this parameter is not present, the default is `python`\.
+ `--class`  —  The Scala class that serves as the entry point for your Scala script\. This only applies if your `--job-language` is set to `scala`\.
+ `--scriptLocation`  —  The S3 location where your ETL script is located \(in a form like `s3://path/to/my/script.py`\)\. This overrides a script location set in the JobCommand object\.
+ `--extra-py-files`  —  S3 path\(s\) to additional Python modules that AWS Glue will add to the Python path before executing your script\. Multiple values must be complete paths separated by a comma \(`,`\)\. Note that only pure Python modules will work currently\. Extension modules written in C or other languages are not supported\.
+ `--extra-jars`  —  S3 path\(s\) to additional Java \.jar file\(s\) that AWS Glue will add to the Java classpath before executing your script\. Multiple values must be complete paths separated by a comma \(`,`\)\.
+ `--extra-files`  —  S3 path\(s\) to additional files such as configuration files that AWS Glue will copy to the working directory of your script before executing it\. Multiple values must be complete paths separated by a comma \(`,`\)\.
+ `--job-bookmark-option`  —  Controls the behavior of a job bookmark\. The following option values can be set:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html)

  For example, to enable a job bookmark, pass the argument:

  ```
  '--job-bookmark-option': 'job-bookmark-enable'
  ```
+ `--TempDir`  —  Specifies an S3 path to a bucket that can be used as a temporary directory for the Job\.

  For example, to set a temporary directory, pass the argument:

  ```
  '--TempDir': 's3-path-to-directory'
  ```

There are also several argument names used by AWS Glue internally that you should never set:
+ `--conf`  —  Internal to AWS Glue\. Do not set\!
+ `--debug`  —  Internal to AWS Glue\. Do not set\!
+ `--mode`  —  Internal to AWS Glue\. Do not set\!
+ `--JOB_NAME`  —  Internal to AWS Glue\. Do not set\!