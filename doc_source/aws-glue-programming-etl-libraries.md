# Developing and Testing ETL Scripts Locally Using the AWS Glue ETL Library<a name="aws-glue-programming-etl-libraries"></a>

The AWS Glue Scala library is available in a public Amazon S3 bucket, and can be consumed by the Apache Maven build system\. This enables you to develop and test your Python and Scala extract, transform, and load \(ETL\) scripts locally, without the need for a network connection\.

Local development is available for AWS Glue versions 0\.9 and 1\.0\. For information about the versions of Python and Apache Spark that are available with AWS Glue, see the [Glue version job property](add-job.md#glue-version-table)\.

The library is released with the Amazon Software license \([https://aws.amazon.com/asl](https://aws.amazon.com/asl)\)\.

**Topics**
+ [Local Development Restrictions](#local-dev-restrictions)
+ [Developing Locally with Python](#develop-local-python)
+ [Developing Locally with Scala](#develop-local-scala)

## Local Development Restrictions<a name="local-dev-restrictions"></a>

Keep the following restrictions in mind when using the AWS Glue Scala library to develop locally\.
+ Avoid creating an assembly jar \("fat jar" or "uber jar"\) with the AWS Glue library, as this will cause the following features to be disabled:
  + [Job bookmarks](https://docs.aws.amazon.com/glue/latest/dg/monitor-continuations.html)
  + Glue Parquet writer \([format="glueparquet"](aws-glue-programming-etl-format.md#aws-glue-programming-etl-format-glue-parquet)\)
  + [FindMatches transform](https://docs.aws.amazon.com/glue/latest/dg/machine-learning.html#find-matches-transform)

  These feature are available only within the AWS Glue job system\.

## Developing Locally with Python<a name="develop-local-python"></a>

Complete some prerequisite tasks and then use AWS Glue utilities to test and submit your Python ETL script\.

### Prerequisites for Local Python Development<a name="prepare-local-python"></a>

Complete these steps to prepare for local Python development:

1. Download the AWS Glue Python library from github \([https://github.com/awslabs/aws-glue-libs](https://github.com/awslabs/aws-glue-libs)\)\.

1. Do one of the following:
   + For Glue version 0\.9, stay on the `master` branch\.
   + For Glue version 1\.0, checkout branch `glue-1.0`\. This version supports Python 3\.

1. Install Apache Maven from the following location: [https://aws-glue-etl-artifacts.s3.amazonaws.com/glue-common/apache-maven-3.6.0-bin.tar.gz](https://aws-glue-etl-artifacts.s3.amazonaws.com/glue-common/apache-maven-3.6.0-bin.tar.gz)\.

1. Install the Apache Spark distribution from one of the following locations:
   + For Glue version 0\.9: [https://aws-glue-etl-artifacts.s3.amazonaws.com/glue-0.9/spark-2.2.1-bin-hadoop2.7.tgz](https://aws-glue-etl-artifacts.s3.amazonaws.com/glue-0.9/spark-2.2.1-bin-hadoop2.7.tgz)
   + For Glue version 1\.0: [https://aws-glue-etl-artifacts.s3.amazonaws.com/glue-1.0/spark-2.4.3-bin-hadoop2.8.tgz](https://aws-glue-etl-artifacts.s3.amazonaws.com/glue-1.0/spark-2.4.3-bin-hadoop2.8.tgz)

1. Export the `SPARK_HOME` environment variable, setting it to the root location extracted from the Spark archive\. For example:
   + For Glue version 0\.9: `export SPARK_HOME=/home/$USER/spark-2.2.1-bin-hadoop2.7`
   + For Glue version 1\.0: `export SPARK_HOME=/home/$USER/spark-2.4.3-bin-spark-2.4.3-bin-hadoop2.8`

### Running Your Python ETL Script<a name="local-run-python-job"></a>

With the AWS Glue jar files available for local development, you can run the AWS Glue Python package locally\.

Use the following utilities and frameworks to test and run your Python script\. The commands listed in the following table are run from the root directory of the AWS Glue Python package \([https://github.com/awslabs/aws-glue-libs](https://github.com/awslabs/aws-glue-libs)\)\.


| Utility | Command | Description | 
| --- | --- | --- | 
| Glue Shell | \./bin/gluepyspark | Enter and run Python scripts in a shell that integrates with AWS Glue ETL libraries\. | 
| Glue Submit | \./bin/gluesparksubmit | Submit a complete Python script for execution\. | 
| Pytest | \./bin/gluepytest | Write and run unit tests of your Python code\. The pytest module must be installed and available in the PATH\. For more information, see [https://docs.pytest.org/en/latest/](https://docs.pytest.org/en/latest/)\. | 

## Developing Locally with Scala<a name="develop-local-scala"></a>

Complete some prerequisite tasks and then issue a Maven command to run your Scala ETL script locally\.

### Prerequisites for Local Scala Development<a name="prepare-local-scala"></a>

Complete these tasks to prepare for local Scala development\.

#### Task 1: Install Software<a name="local-scala-prereqs-task1"></a>

In this task you install software and set the required environment variable\.

1. Install Apache Maven from the following location: [https://aws-glue-etl-artifacts.s3.amazonaws.com/glue-common/apache-maven-3.6.0-bin.tar.gz](https://aws-glue-etl-artifacts.s3.amazonaws.com/glue-common/apache-maven-3.6.0-bin.tar.gz)\.

1. Install the Apache Spark distribution from one of the following locations:
   + For Glue version 0\.9: [https://aws-glue-etl-artifacts.s3.amazonaws.com/glue-0.9/spark-2.2.1-bin-hadoop2.7.tgz](https://aws-glue-etl-artifacts.s3.amazonaws.com/glue-0.9/spark-2.2.1-bin-hadoop2.7.tgz)
   + For Glue version 1\.0: [https://aws-glue-etl-artifacts.s3.amazonaws.com/glue-1.0/spark-2.4.3-bin-hadoop2.8.tgz](https://aws-glue-etl-artifacts.s3.amazonaws.com/glue-1.0/spark-2.4.3-bin-hadoop2.8.tgz)

1. Export the `SPARK_HOME` environment variable, setting it to the root location extracted from the Spark archive\. For example:
   + For Glue version 0\.9: `export SPARK_HOME=/home/$USER/spark-2.2.1-bin-hadoop2.7`
   + For Glue version 1\.0: `export SPARK_HOME=/home/$USER/spark-2.4.3-bin-spark-2.4.3-bin-hadoop2.8`

#### Task 2: Configure Your Maven Project<a name="local-scala-prereqs-task2"></a>

Use the following `pom.xml` file as a template for your AWS Glue Scala applications\. It contains the required `dependencies`, `repositories`, and `plugins` elements\. Replace the `Glue version` string with `1.0.0` for Glue version 1\.0 or `0.9.0` for Glue version 0\.9\.

```
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.amazonaws</groupId>
    <artifactId>AWSGlueApp</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>${project.artifactId}</name>
    <description>AWS Glue ETL application</description>

        <properties>
        <scala.version>2.11.1</scala.version>
        </properties>

    <dependencies>
        <dependency>
            <groupId>org.scala-lang</groupId>
            <artifactId>scala-library</artifactId>
            <version>${scala.version}</version>
        </dependency>
        <dependency>
            <groupId>com.amazonaws</groupId>
            <artifactId>AWSGlueETL</artifactId>
			
            <version>Glue version</version>
			
        </dependency>
    </dependencies>

    <repositories>
        <repository>
            <id>aws-glue-etl-artifacts</id>
            <url>https://aws-glue-etl-artifacts.s3.amazonaws.com/release/</url>
        </repository>
    </repositories>
    <build>
        <sourceDirectory>src/main/scala</sourceDirectory>
        <plugins>
            <plugin>
                <!-- see http://davidb.github.com/scala-maven-plugin -->
                <groupId>net.alchim31.maven</groupId>
                <artifactId>scala-maven-plugin</artifactId>
                <version>3.4.0</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>compile</goal>
                            <goal>testCompile</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.6.0</version>
                <executions>
                    <execution>
                        <goals>
                        <goal>java</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                <systemProperties>
                    <systemProperty>
                        <key>spark.master</key>
                        <value>local[*]</value>
                    </systemProperty>
                    <systemProperty>
                        <key>spark.app.name</key>
                        <value>localrun</value>
                    </systemProperty>
                    <systemProperty>
                        <key>org.xerial.snappy.lib.name</key>
                        <value>libsnappyjava.jnilib</value>
                    </systemProperty>
                </systemProperties>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-enforcer-plugin</artifactId>
                <version>3.0.0-M2</version>
                <executions>
                    <execution>
                        <id>enforce-maven</id>
                        <goals>
                            <goal>enforce</goal>
                        </goals>
                        <configuration>
                            <rules>
                                <requireMavenVersion>
                                    <version>3.5.3</version>
                                </requireMavenVersion>
                            </rules>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

### Running Your Scala ETL Script<a name="local-run-scala-job"></a>

Run the following command from the Maven project root directory to execute your Scala ETL script:

```
mvn exec:java -Dexec.mainClass="mainClass" -Dexec.args="--JOB-NAME jobName"
```

Replace *mainClass* with the fully qualified class name of the script's main class\. Replace *jobName* with the desired job name\.