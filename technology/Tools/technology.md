---
title: Installing Apache Spark
description: Some notes for myself as a reminder.  What to install, where to get extra libraries to read from S3.
published: true
date: 2022-01-03T19:03:31.869Z
tags: 
editor: markdown
dateCreated: 2022-01-03T19:03:31.869Z
---

# Apache Spark
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