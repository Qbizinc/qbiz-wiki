---
title: Definition of Hive Type
description: 
published: true
date: 2021-08-29T22:24:14.933Z
tags: 
editor: markdown
dateCreated: 2021-08-11T21:01:11.881Z
---

# What
Hadoop MapReduce was the first open-source Big Data query engine. It spawned a new open-source, standards-based architecture for distributed, separate-storage-and-compute read query engines.

[Hive](/training/qram/nibbles/definition_of_hive) was developed later under the Hadoop umbrella to provide a SQL read query interface to Hadoop MapReduce.

Multiple newer SQL-based query engines are designed to be compatible with this architecture to ease adoption into existing Hadoop environments.

Qbiz categorizes this type of architecture as "Hive-type". (The term "Hive-type" is not in common usage.)

## Query Engines
- Hive (on Hadoop MapReduce)
- Hive (on Tez)
- Presto
- Impala
- Spark (on Hive Metastore)

## Components
- Storage layer in a [file system allowing for reading byte-offset slices of files](/training/qram/nibbles/definition_of_hdfs)
- Files in [self-describing slice-oriented file format](/training/qram/dishes/slice_oriented_file_format)
- Use of Hive Metastore:
  - Schema/table/partition object types
  - "One location per partition"
- Compute layer using the above
