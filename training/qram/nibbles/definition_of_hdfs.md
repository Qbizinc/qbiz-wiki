---
title: Definition of HDFS
description: 
published: true
date: 2021-08-20T16:28:29.880Z
tags: 
editor: markdown
dateCreated: 2021-08-10T19:35:56.434Z
---

# What
Hadoop Distributed File System (HDFS) is a distributed file system implemented in Java originally to support Hadoop MapReduce. Therefore, it was architected to meet that specific need. Other query engines adapt themselves to this architecture.

## Components
- Name Node (file system metadata)
  - Single node, no failover
- Secondary Name Node (file system metadata assistant)
  - Computes the file system metadata data structure at well-known checkpoints, providing some protection against data loss and removing this expensive task from the Name Node
- Data Node (physical blocks)
  - Provides actual storage and sends heartbeats every 3 seconds to the Name Node.
  - If the Name Node doesn't receive a heartbeat (either the Data Node is down or there is a network interruption), the Name Node assums the DataNode is dead and orders the remaining Data Nodes to start copying blocks amongst themselves to ensure the replication factor.

## API

### Slices
- [Perhaps the most important feature is the ability to read only slices of files.](/training/qram/dishes/slice_oriented_file_format)
- [Hive-type query engines depend on this feature.](/training/qram/nibbles/definition_of_hive_type)

### Java
Processes in the Hadoop ecosystem use the corresponding Java libraries to the HDFS version.

A command-line utility implemented in Java using those same libraries is available if installed on a user environment. This command-line utility provides a somewhat-Unix-like CLI to the file system. Because it is implemented in Java, startup is slow; it is launching and shutting down a Java VM with every command.

## Blocks
Default file system block size is 128 MB.

This can be configured in `core-site.xml`.

**NB:** Changes to this setting do not rewrite existing files!

# References
- [Wikipedia](https://en.wikipedia.org/wiki/Apache_Hadoop#HDFS)
- [HDFS v3.0.0 Architecture](https://hadoop.apache.org/docs/r3.0.0/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html)
