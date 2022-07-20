---
title: Installing Apache Spark
description: Some notes for myself as a reminder.  What to install, where to get extra libraries to read from S3.
published: true
date: 2022-07-20T22:58:56.164Z
tags: 
editor: markdown
dateCreated: 2022-01-03T19:03:31.869Z
---

# Apache Spark and adding Glue libraries
## Installation
Got to *https://spark.apache.org/downloads.html* and download the version that is pre-built with Apache Hadoop (unless you plan to leverage and existing hadoop cluster).  If installing on a remote Linux server, copy the download link and use *wget* to download the package.

Once you've unpackaged the compressed tar file, you will need to download. some additional Jars if you want to connect to S3.  A Google search for the following libraries should lead you to a Maven repository where you can download the jars.  Make sure you download the same version as the hadoop libaries included in you Apache Spark package.  For instance, at the time I did this, the version is 3.3.1.

Additional Libareries
* hadoop-aws
* hadoop-common

For example...
```bash
9archi0@ds-cuda-prod:~/tmp/spark-3.2.0-bin-hadoop3.2/jars$ ls -l hadoop-*
-rw-r--r-- 1 9archi0 9archi0 19406393 Oct  6 13:18 hadoop-client-api-3.3.1.jar
-rw-r--r-- 1 9archi0 9archi0 31717292 Oct  6 13:18 hadoop-client-runtime-3.3.1.jar
-rw-r--r-- 1 9archi0 9archi0  3362359 Oct  6 13:18 hadoop-shaded-guava-1.1.1.jar
-rw-r--r-- 1 9archi0 9archi0    56507 Oct  6 13:18 hadoop-yarn-server-web-proxy-3.3.1.jar
```
After downloading additional libaries...
```bash
9archi0@ds-cuda-prod:~/spark-3.2.0-bin-hadoop3.2/jars$ ls -l hadoop-*
-rw-rw-r-- 1 9archi0 9archi0   870644 Jun  1  2021 hadoop-aws-3.3.1.jar
-rw-r--r-- 1 9archi0 9archi0 19406393 Oct  6 13:18 hadoop-client-api-3.3.1.jar
-rw-r--r-- 1 9archi0 9archi0 31717292 Oct  6 13:18 hadoop-client-runtime-3.3.1.jar
-rw-rw-r-- 1 9archi0 9archi0  4426299 Jun  1  2021 hadoop-common-3.3.1.jar
-rw-r--r-- 1 9archi0 9archi0  3362359 Oct  6 13:18 hadoop-shaded-guava-1.1.1.jar
-rw-r--r-- 1 9archi0 9archi0    56507 Oct  6 13:18 hadoop-yarn-server-web-proxy-3.3.1.jar
```
Getting Hadoop and AWS JARs
```bash
wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.3.1/hadoop-aws-3.3.1.jar; \
wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-common/3.3.1/hadoop-common-3.3.1.jar; \
wget https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk/1.12.136/aws-java-sdk-1.12.136.jar; \
wget https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-core/1.12.136/aws-java-sdk-core-1.12.136.jar; \
wget https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-s3/1.12.136/aws-java-sdk-s3-1.12.136.jar; \
wget https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-dynamodb/1.12.136/aws-java-sdk-dynamodb-1.12.136.jar
```
## Adding Glue libraries
Follow the instructions found in AWS' Documentation for [Developing and Testing ETL Scripts Locally Using the AWS Glue ETL Library](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-libraries.html#develop-local-python).

### Important Notes
- Depending on the age of your OS, your version of Maven may be too old to run pull the dependencies with the provided `pom.xml`.
- The instructions are insufficent for Maven newbies to run without needing to google the solution.  Use `cd aws-glue-libs; mvn install dependency:copy-dependencies`.
- Glue v3, designed to work with Spark v3.x, is designed to run on Amazon Linux.  I had issues installing it on Ubuntu, but there are so many things than can go wrong, I may have made some error.
- Glue v2 requires Python 3.7.x, which seems to use standard Apache Spark. 
- there is a bug in the AWS repo and these directions must be followed to repair: https://support.wharton.upenn.edu/help/glue-debugging#run-glue-setup-sh

## Adding Rapids Libraries
Rapids is a Spark Plugin (supported as of version 3.0) that accelerates Spark SQL, ML operations and dataframe operations using GPUs.  See the [Rapids Homepage](https://nvidia.github.io/spark-rapids/) for more details.

In short, GPUs can accelerate many Spark tasks.  However, there is overhead in moving data between main memory and GPU memory so you may see performance degredation in short jobs where moving data in and out of main memory outweighs the cost of processing the data with CPUs.

Follow these steps to enable GPUs with Rapids:
-	Download RAPIDS JAR, its dependency cuDF JAR, and the CUDA discovery script.  The JAR versions must be compatible with your CUDA driver versions.  Set the environment variables as specified int he documentation and add update the `spark-env.sh` configuration file.
- Launch your script or REPL with the flags that tell you want to use GPUs and the RAPIDS plugin.
e.g.
```
$SPARK_HOME/bin/pyspark --master local[*] \
    --conf spark.rapids.sql.concurrentGpuTasks=1 \
    --conf spark.rapids.memory.pinnedPool.size=4G \
    --conf spark.plugins=com.nvidia.spark.SQLPlugin \
    --jars ${SPARK_CUDF_JAR},${SPARK_RAPIDS_PLUGIN_JAR}
```
At this time, it seems that Rapids is available in AWS but only as part of EMR -- [Use the Nvidia Spark-RAPIDS Accelerator for Spark](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-spark-rapids.html).

## Other important links
- Youtube by Databricks regarding Spark 3.0 features: https://www.youtube.com/watch?v=4MI_LYah900
- Spark performance monitoring (2019): https://www.youtube.com/watch?v=daXEp4HmS-E
- https://docs.rapids.ai/api/cudf/nightly/user_guide/10min.html#Object-Creation
- https://medium.com/walmartglobaltech/getting-started-with-apache-spark-gpu-rapids-part-i-938664771092
- [Deep Dive into the New Features of Apache Spark 3.2 and 3.3](https://www.youtube.com/watch?v=CZWYKRkXhy8) Data+AI Summit 2022.
- [From Query Plan to Performance](https://www.youtube.com/watch?v=_Ne27JcLnEc)
    - @49:40, nice diagram of join decision tree.

## Things to think about and review
- Small files are the death of Shuffles!

## Glossary
- Wide Operations -- require an exchangev (Shuffle).
- Narrow Operations -- don't require an exchnage (Shuffle).

## Physical Plans
* Exchange Operator -- A shuggle.
    - Shuffle Exchange -- large data sets are written to local drives then read by next operation, required when data needs to be redistributed.
    - Broadcast Echange -- small data sets are sent to Drive node and then sent to all Executors.
    
* Join Types (See 46:11 mark of [From Query Plan to Performance](https://www.youtube.com/watch?v=_Ne27JcLnEc))
    - SortMergeJoinExec
    - BroadcastHashJoinExec
    - BroadcastNestedLoopJoinExec
    - CartesianProductExec
    
* Partial Aggregations -- can be very slow for lots of distinct key values.  An aggregation operator in the _Map_ phase.  Can use Spark Configuration parameter (spark.sql.aggregate.partialaggregare.skip.enabled=true).  