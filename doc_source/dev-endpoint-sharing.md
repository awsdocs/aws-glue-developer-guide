# Advanced Configuration: Sharing Development Endpoints among Multiple Users<a name="dev-endpoint-sharing"></a>

This section explains how you can take advantage of development endpoints with SageMaker notebooks in typical use cases to share development endpoints among multiple users\.

## Single\-tenancy Configuration<a name="dev-endpoint-sharing-sharing-single"></a>

In single tenant use\-cases, to simplify the developer experience and to avoid contention for resources it is recommended that you have each developer use their own development endpoint sized for the project they are working on\. This also simplifies the decisions related to worker type and DPU count leaving them up to the discretion of the developer and project they are working on\. 

You won’t need to take care of resource allocation unless you runs multiple notebook files concurrently\. If you run code in multiple notebook files at the same time, multiple Livy sessions will be launched concurrently\. To segregate Spark cluster configurations in order to run multiple Livy sessions at the same time, you can follow the steps which are introduced in multi tenant use\-cases\.

## Multi\-tenancy Configuration<a name="dev-endpoint-sharing-sharing-multi"></a>

**Note**  
Please note, development endpoints are intended to emulate the AWS Glue ETL environment as a single\-tenant environment\. While multi\-tenant use is possible, it is an advanced use\-case and it is recommended most users maintain a pattern of single\-tenancy for each development endpoint\.

In multi tenant use\-cases, you might need to take care of resource allocation\. The key factor is the number of concurrent users who use a Jupyter notebook at the same time\. If your team works in a "follow\-the\-sun" workflow and there is only one Jupyter user at each time zone, then the number of concurrent users is only one, so you won’t need to be concerned with resource allocation\. However, if your notebook is shared among multiple users and each user submits code in an ad\-hoc basis, then you will need to consider the below points\.

To segregate Spark cluster resources among multiple users, you can use SparkMagic configurations\. There are two different ways to configure SparkMagic\.

### \(A\) Use the %%configure \-f Directive<a name="dev-endpoint-sharing-sharing-multi-a"></a>

If you want to modify the configuration per Livy session from the notebook, you can run the `%%configure -f` directive on the notebook paragraph\.

For example, if you want to run Spark application on 5 executors, you can run the following command on the notebook paragraph\.

```
%%configure -f
{"numExecutors":5}
```

Then you will see only 5 executors running for the job on the Spark UI\.

We recommend limiting the maximum number of executors for dynamic resource allocation\.

```
%%configure -f
{"conf":{"spark.dynamicAllocation.maxExecutors":"5"}}
```

### \(B\) Modify the SparkMagic Config File<a name="dev-endpoint-sharing-sharing-multi-b"></a>

SparkMagic works based on the [Livy API](https://livy.incubator.apache.org/docs/latest/rest-api.html)\. SparkMagic creates Livy sessions with configurations such as `driverMemory`, `driverCores`, `executorMemory`, `executorCores`, `numExecutors`, `conf`, etc\. Those are the key factors that determine how much resources are consumed from the entire Spark cluster\. SparkMagic allows you to provide a config file to specify those parameters which are sent to Livy\. You can see a sample config file in this [Github repository](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json)\.

If you want to modify configuration across all the Livy sessions from a notebook, you can modify `/home/ec2-user/.sparkmagic/config.json` to add `session_config`\.

To modify the config file on a SageMaker notebook instance, you can follow these steps\.

1. Open a SageMaker notebook\.

1. Open the Terminal kernel\.

1. Run the following commands:

   ```
   sh-4.2$ cd .sparkmagic
   sh-4.2$ ls
   config.json logs
   sh-4.2$ sudo vim config.json
   ```

   For example, you can add these lines to `/home/ec2-user/.sparkmagic/config.json` and restart the Jupyter kernel from the notebook\.

   ```
     "session_configs": {
       "conf": {
         "spark.dynamicAllocation.maxExecutors":"5"
       }
     },
   ```

## Guidelines and Best Practices<a name="dev-endpoint-sharing-sharing-guidelines"></a>

To avoid this kind of resource conflict, you can use some basic approaches like:
+ Have a larger Spark cluster by increasing the `NumberOfWorkers` \(scaling horizontally\) and upgrading the `workerType` \(scaling vertically\)
+ Allocate fewer resources per user \(fewer resources per Livy session\)

Your approach will depend on your use case\. If you have a larger development endpoint, and there is not a huge amount of data, the possibility of a resource conflict will decrease significantly because Spark can allocate resources based on a dynamic allocation strategy\.

As described above, the number of Spark executors can be automatically calculated based on a combination of DPU \(or `NumberOfWorkers`\) and worker type\. Each Spark application launches one driver and multiple executors\. To calculate you will need the `NumberOfWorkers` = `NumberOfExecutors + 1`\. The matrix below explains how much capacity you need in your development endpoint based on the number of concurrent users\.


****  

| Number of concurrent notebook users | Number of Spark executors you want to allocate per user | Total NumberOfWorkers for your dev endpoint | 
| --- | --- | --- | 
| 3 | 5 | 18 | 
| 10 | 5 | 60 | 
| 50 | 5 | 300 | 

If you want to allocate fewer resources per user, the `spark.dynamicAllocation.maxExecutors` \(or `numExecutors`\) would be the easiest parameter to configure as a Livy session parameter\. If you set the below configuration in `/home/ec2-user/.sparkmagic/config.json`, then SparkMagic will assign a maximum of 5 executors per Livy session\. This will help segregating resources per Livy session\.

```
"session_configs": {
    "conf": {
      "spark.dynamicAllocation.maxExecutors":"5"
    }
  },
```

Suppose there is a dev endpoint with 18 workers \(G\.1X\) and there are 3 concurrent notebook users at the same time\. If your session config has `spark.dynamicAllocation.maxExecutors=5` then each user can make use of 1 driver and 5 executors\. There won’t be any resource conflicts even when you run multiple notebook paragraphs at the same time\.

### Trade\-offs<a name="dev-endpoint-sharing-sharing-multi-tradeoffs"></a>

With this session config `"spark.dynamicAllocation.maxExecutors":"5"`, you will be able to avoid resource conflict errors and you do not need to wait for resource allocation when there are concurrent user accesses\. However, even when there are many free resources \(for example, there are no other concurrent users\), Spark cannot assign more than 5 executors for your Livy session\.

### Other Notes<a name="dev-endpoint-sharing-sharing-multi-notes"></a>

It is a good practice to stop the Jupyter kernel when you stop using a notebook\. This will free resources and other notebook users can use those resources immediately without waiting for kernel expiration \(auto\-shutdown\)\.

## Common Issues<a name="dev-endpoint-sharing-sharing-issues"></a>

Even when following the guidelines, you may experience certain issues\.

### Session not found<a name="dev-endpoint-sharing-sharing-issues-session"></a>

When you try to run a notebook paragraph even though your Livy session has been already terminated, you will see the below message\. To activate the Livy session, you need to restart the Jupyter kernel by choosing **Kernel** > **Restart** in the Jupyter menu, then run the notebook paragraph again\.

```
An error was encountered:
Invalid status code '404' from http://localhost:8998/sessions/13 with error payload: "Session '13' not found."
```

### Not enough YARN resources<a name="dev-endpoint-sharing-sharing-issues-yarn-resources"></a>

When you try to run a notebook paragraph even though your Spark cluster does not have enough resources to start a new Livy session, you will see the below message\. You can often avoid this issue by following the guidelines, however, there might be a possibility that you face this issue\. To workaround the issue, you can check if there are any unneeded, active Livy sessions\. If there are unneeded Livy sessions, you will need to terminate them to free the cluster resources\. See the next section for details\.

```
Warning: The Spark session does not have enough YARN resources to start. 
The code failed because of a fatal error:
    Session 16 did not start up in 60 seconds..

Some things to try:
a) Make sure Spark has enough available resources for Jupyter to create a Spark context.
b) Contact your Jupyter administrator to make sure the Spark magics library is configured correctly.
c) Restart the kernel.
```

## Monitoring and Debugging<a name="dev-endpoint-sharing-sharing-debugging"></a>

This section describes techniques for monitoring resources and sessions\.

### Monitoring and Debugging Cluster Resource Allocation<a name="dev-endpoint-sharing-sharing-debugging-a"></a>

You can watch the Spark UI to monitor how many resources are allocated per Livy session, and what are the effective Spark configurations on the job\. To enable the Spark UI, see [Enabling the Apache Spark Web UI for Development Endpoints](https://docs.aws.amazon.com/glue/latest/dg/monitor-spark-ui-dev-endpoints.html)\.

\(Optional\) If you need a real\-time view of the Spark UI, you can configure an SSH tunnel against the Spark history server running on the Spark cluster\.

```
ssh -i <private-key.pem> -N -L 8157:<development endpoint public address>:18080 glue@<development endpoint public address>
```

You can then open http://localhost:8157 on your browser to view the Spark UI\.

### Free Unneeded Livy Sessions<a name="dev-endpoint-sharing-sharing-debugging-b"></a>

Review these procedures to shut down any unneeded Livy sessions from a notebook or a Spark cluster\.

**\(a\)\. Terminate Livy sessions from a notebook**  
You can shut down the kernel on a Jupyter notebook to terminate unneeded Livy sessions\.

**\(b\)\. Terminate Livy sessions from a Spark cluster**  
If there are unneeded Livy sessions which are still running, you can shut down the Livy sessions on the Spark cluster\.

As a pre\-requisite to perform this procedure, you need to configure your SSH public key for your development endpoint\.

To log in to the Spark cluster, you can run the following command:

```
$ ssh -i <private-key.pem> glue@<development endpoint public address>
```

You can run the following command to see the active Livy sessions:

```
$ yarn application -list
20/09/25 06:22:21 INFO client.RMProxy: Connecting to ResourceManager at ip-255-1-106-206.ec2.internal/172.38.106.206:8032
Total number of applications (application-types: [] and states: [SUBMITTED, ACCEPTED, RUNNING]):2
Application-Id Application-Name Application-Type User Queue State Final-State Progress Tracking-URL
application_1601003432160_0005 livy-session-4 SPARK livy default RUNNING UNDEFINED 10% http://ip-255-1-4-130.ec2.internal:41867
application_1601003432160_0004 livy-session-3 SPARK livy default RUNNING UNDEFINED 10% http://ip-255-1-179-185.ec2.internal:33727
```

You can then shut down the Livy session with the following command:

```
$ yarn application -kill application_1601003432160_0005
20/09/25 06:23:38 INFO client.RMProxy: Connecting to ResourceManager at ip-255-1-106-206.ec2.internal/255.1.106.206:8032
Killing application application_1601003432160_0005
20/09/25 06:23:39 INFO impl.YarnClientImpl: Killed application application_1601003432160_0005
```