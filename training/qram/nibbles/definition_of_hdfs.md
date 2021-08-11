---
title: Definition of HDFS
description: 
published: true
date: 2021-08-11T21:28:22.061Z
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
Perhaps the most important feature is the ability to read only slices of files.

For example:
> 1. A file is 200 MB.
> 2. The file has a format with sections of defined lengths:
>    1. Parquet files, the first and last 4 bytes say "PAR1" in utf-8.
>    2. At the very end of the file, before the final "PAR1", are 4 bytes in little endian for the number of bytes for the length of file metadata.
> 3. The file metadata is right before the 4 bytes for length.
> 4. A reader would read:
>    1. First 4 bytes (confirm this is "PAR1")
>    2. Last 8 bytes (confirm this is "PAR1" and get the length of the file metadata
>    3. Length of file metadata before last 8 bytes
> 5. A query engine could then parse the file metadata to determine how to get specific columns

### Java
Processes in the Hadoop ecosystem use the corresponding Java libraries to the HDFS version.

A command-line utility implemented in Java using those same libraries is available if installed on a user environment. This command-line utility provides a somewhat-Unix-like CLI to the file system. Because it is implemented in Java, startup is slow; it is launching and shutting down a Java VM with every command.

## Blocks
Default block size is 128 MB.

# References
- [Wikipedia](https://en.wikipedia.org/wiki/Apache_Hadoop#HDFS)
- [HDFS v3.0.0 Architecture](https://hadoop.apache.org/docs/r3.0.0/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html)
