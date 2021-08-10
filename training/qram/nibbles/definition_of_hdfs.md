---
title: Definition of HDFS
description: 
published: true
date: 2021-08-10T19:35:56.434Z
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

## Blocks
Default block size is 128 MB.

## API
Processes in the Hadoop ecosystem use the corresponding Java libraries to the HDFS version.

A command-line utility implemented in Java using those same libraries is available if installed on a user environment. This command-line utility provides a somewhat-Unix-like CLI to the file system. Because it is implemented in Java, startup is slow; it is launching and shutting down a Java VM with every command.

# References
- [Wikipedia](https://en.wikipedia.org/wiki/Apache_Hadoop#HDFS)
- [HDFS v3.0.0 Architecture](https://hadoop.apache.org/docs/r3.0.0/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html)
