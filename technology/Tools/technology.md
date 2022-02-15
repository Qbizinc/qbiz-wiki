---
title: Installing Apache Spark
description: Some notes for myself as a reminder.  What to install, where to get extra libraries to read from S3.
published: true
date: 2022-01-27T16:28:41.740Z
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