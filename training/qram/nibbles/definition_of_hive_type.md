---
title: Definition of Hive Type
description: 
published: true
date: 2021-08-12T21:48:57.427Z
tags: 
editor: markdown
dateCreated: 2021-08-11T21:01:11.881Z
---

# What
Hadoop MapReduce was the first open-source Big Data query engine. It spawned a new open-source, standards-based architecture for distributed, separate-storage-and-compute read query engines.

Hive was developed later under the Hadoop umbrella to provide a SQL read query interface to Hadoop MapReduce.

Because of the open-source, standards-based nature of Hive, newer SQL-based query engines are designed to be compatible with this architecture.

Qbiz categorizes this type of architecture as "Hive-type". (The term "Hive-type" is not in common usage.)

## Components
- Storage layer in the style of HDFS
  - Allows for reading byte-offset slices of files
- Use of Hive Metastore (optional)
  - Schema/table/partition object types
  - "One location per partition"
- Compute layer using HMS (optional) and HDFS-type storage

## Pros and Cons

### Pros
- Can merely add the new query engine compute layer without disturbing existing storage.
  - Data copying/moving is expensive (i.e. data gravity).
  - No need to host separate copies of data for each query engine
  - Quick adoption because lower DevOps resistance
- If sharing Hive Metastore, no work to port schema/table/partition definitions for new query engine
  - Also, changes to HMS by one query engine are immediately synced to other query engines

### Cons
- Can 
