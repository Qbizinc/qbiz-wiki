---
title: Definition of Hive
description: 
published: true
date: 2021-08-29T22:15:22.306Z
tags: 
editor: markdown
dateCreated: 2021-08-11T03:03:28.282Z
---

# What
Hive was created to allow users to write SQL queries instead of Java in Hadoop MapReduce, while still using Hadoop MapReduce infrastructure for its horizontal scale and fault tolerance.

Originally, Hive compiled SQL to Hadoop MapReduce. Hive has since moved onto [Tez](https://tez.apache.org/), which works around a key architectural limitation of Hadoop MapReduce.

Hive SQL is not ANSI-compatible with any year-standard. Most of the limitations are based on the file-oriented nature of Hadoop.

## Hive Metastore
In order to compile SQL to Hadoop MR or Tez, Hive maintains the relational table definitions. HDFS and Hadoop MR / Tez are file-aware; they do not have tables. Therefore, Hive keeps metadata of the Hive relational database (list of tables, etc.) as well as which HDFS paths they are actually stored as. This is the [Hive Metastore (HMS)](https://docs.cloudera.com/runtime/7.2.10/hive-hms-overview/topics/hive-hms-introduction.html).

# File Oriented Nature
1. Hive uses Hadoop MapReduce or Tez, both of which use HDFS.
2. HDFS hosts files with a default block size of 128 MB.

Therefore, transaction processing (OLTP) operations such as single record INSERTs, UPDATEs, or DELETEs are not possible.

(They are technically possible with some unrelated extensions to the architecture.)

# Self-Describing Files
The HMS contains no file-level metadata.
1. Hive reads the HMS to determine which partitions to read for a given query.
2. Those partitions each have an HDFS path.
3. Hive must read from the HDFS NameNode the list of files that belong to that HDFS path.
4. Hive then examines each file to see if they should be processed further.

Because of the architectural decision to not include file metadata in the HMS, HDFS does much more work. The benefit is avoiding the file metadata outside of files to become out-of-sync with the files themselves.

[HDFS supports reading by slice, without which self-describing files would be impractical.](/training/qram/dishes/slice_oriented_file_format)

# References
- [Wikipedia](https://en.wikipedia.org/wiki/Apache_Hive)
- [Design](https://cwiki.apache.org/confluence/display/hive/design)
- [Tez](https://tez.apache.org/)
- [Hive Metastore](https://docs.cloudera.com/runtime/7.2.10/hive-hms-overview/topics/hive-hms-introduction.html)
