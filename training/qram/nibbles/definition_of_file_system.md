---
title: Definition of File System
description: 
published: true
date: 2021-08-10T18:56:21.241Z
tags: 
editor: markdown
dateCreated: 2021-08-10T18:56:21.241Z
---

# What
A system to control how data is stored and retrieved.

## Components
- Logical file system: provides an API to a user/process to interact with the file system
- Virtual file system (optional): allows multiple physical file systems to be interacted with as if belonging to a single file system
- Physical file system: physical operation of the storage medium
  - Blocks
  - Buffering
  - Memory management
  - Placement of data to blocks
  - Interacts with device drivers or channels to drive the storage device
  
## Blocks
Storage devices typically do not provide addressing at the byte-level. Therefore, they are grouped into blocks. Also, random-access of bytes is typically against the physics of many types of storage devices. Blocking helps provide performance guarantees.

## Metadata
File systems maintain a **table of contents** mapping **file names** to the specific *physical file system blocks* that make up the file contents.

The metadata system may attach metadata to the file. Higher-level constructs like directories/folders are common, and these constructs may have their own metadata.

Most file systems use the Unix file name and directory conventions.

## Distributed File Systems
Distributed file systems exist primarily for two purposes:
- Increased data storage (the disks on one node are not enough)
- Resiliency (protect against data loss or provide failover to ensure service-level)

### Data Loss
Protecting against data loss usually requires saving multiple copies of the same data. In distributed file systems, this means multiple copies of the same blocks saved on different storage nodes.

In Hadoop Distributed File System (HDFS), the default block size is 128 MB. This implies that files with a byte-size that is not an integer multiple of 128 MB have empty space at the end.
- In distributed file systems like HDFS, blocks are saved multiple times across different nodes, based on a replication factor
  - This guards against data loss.
  - This may also avoid node hotspots (though in practice few applications take advantage)
  - In HDFS, these nodes are called DataNodes. The NameNode holds the **table of contents** that points file names to the specific blocks on their DataNodes.

# References
- [Wikipedia](https://en.wikipedia.org/wiki/File_system)
- [Wikipedia: Blocks](https://en.wikipedia.org/wiki/Block_(data_storage))
- [HDFS v3.0.0 Architecture](https://hadoop.apache.org/docs/r3.0.0/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html)
