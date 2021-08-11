---
title: Definition of Hive
description: 
published: true
date: 2021-08-11T03:11:00.056Z
tags: 
editor: markdown
dateCreated: 2021-08-11T03:03:28.282Z
---

# What
Hive was created to allow users to write SQL queries instead of Java in Hadoop MapReduce, while still using Hadoop MapReduce infrastructure for its horizontal scale and fault tolerance.

Originally, Hive compiled SQL to Hadoop MapReduce. Hive has since moved onto [Tez](https://tez.apache.org/), which works around a key architectural limitation of Hadoop MapReduce.

## Hive Metastore
In order to compile SQL to Hadoop MR or Tez, Hive maintains the relational table definitions. HDFS is file-aware; it does not have tables. Therefore, Hive keeps metadata of the Hive relational database (list of tables, etc.) as well as which HDFS files they are actually stored as. This is the [Hive Metastore](https://docs.cloudera.com/runtime/7.2.10/hive-hms-overview/topics/hive-hms-introduction.html)

# References
- [Wikipedia](https://en.wikipedia.org/wiki/Apache_Hive)
- [Design](https://cwiki.apache.org/confluence/display/hive/design)
- [Tez](https://tez.apache.org/)
- [Hive Metastore](https://docs.cloudera.com/runtime/7.2.10/hive-hms-overview/topics/hive-hms-introduction.html)
