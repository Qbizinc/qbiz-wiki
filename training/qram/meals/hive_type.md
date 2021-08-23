---
title: Fundamentals of Hive Type
description: 
published: true
date: 2021-08-23T14:38:18.846Z
tags: 
editor: markdown
dateCreated: 2021-08-23T14:17:11.210Z
---

# What
Hive-Type architecture does not only apply to Hive. Other query engines have adopted it; open-source ecosystems favor compatibility:
- Presto
- Impala
- Spark

This [meal](/training/qram/meals) introduces the fundamentals of the Hive-Type architecture and set the stage for query performance tuning, DevOps, and service architecture.

# Path
1. Hive-Type is defined by HDFS-style storage and Hive Metastore (HMS) type relational schema storage.
2. First, learn about HDFS:
   1. [Overview of Hadoop Distributed File System](/training/qram/dishes/overview_of_hdfs)
   2. Note the lack of versioning in HDFS.
      - HMS and query engines cannot use HDFS versioning.
   3. Note also that journaling is optional.
      - HMS and query engines cannot use journaling.
