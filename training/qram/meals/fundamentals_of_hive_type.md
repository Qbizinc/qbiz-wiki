---
title: Fundamentals of Hive Type
description: 
published: true
date: 2021-08-29T22:26:40.580Z
tags: 
editor: markdown
dateCreated: 2021-08-23T14:17:11.210Z
---

# What
Hive-Type architecture does not only apply to Hive. Other query engines have adopted it; open-source ecosystems favor compatibility:
- Presto
- Impala
- Spark

This introduces the fundamentals of the Hive-Type architecture and set the stage for query performance tuning, service architecture, and DevOps.

# Path
1. Hive-Type is defined by HDFS-style storage and Hive Metastore (HMS) type relational schema storage.
2. First, learn about HDFS:
   1. [Overview of Hadoop Distributed File System (Dish)](/training/qram/dishes/overview_of_hdfs)
   2. Note the lack of versioning in HDFS.
   3. Note also that journaling is optional and is inaccessible to users.
3. Next, learn about Hive Metastore (HMS):
   1. [Overview of Hive Metastore (Dish)](/training/qram/dishes/overview_of_hms)
   2. Note the single-location-per-partition property.
4. Next, learn about slice-oriented file formats:
   1. [Slice Oriented File Format (Dish)](/training/qram/dishes/slice_oriented_file_format)
   2. Note the need to query both HDFS NameNode and DataNode to get file metadata.
5. Next, learn about Hive:
   1. [Definition of Hive (Nibble)](/training/qram/nibbles/definition_of_hive)
   2. Note the lack of OLTP-type operations.
   - (WIP) Expand on Hive to how it compiles to both Hadoop MR and Tez.
6. Finally, learn about Hive-type:
   1. [Overview of Hive Type (Dish)](/training/qram/dishes/overview_of_hive_type)

## Optional
1. (WIP) Presto
2. (WIP) Impala
3. (WIP) Slack on Hive Metastore
