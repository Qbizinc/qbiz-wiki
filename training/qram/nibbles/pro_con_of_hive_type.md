---
title: Pros and Cons of Hive Type
description: 
published: true
date: 2021-08-29T22:23:32.062Z
tags: 
editor: markdown
dateCreated: 2021-08-23T19:58:25.278Z
---

# What
See [Definition of Hive Type](/training/qram/nibbles/definition_of_hive_type).

# Pros

## Adoption
For example, an organization would like to deploy Presto alongside an existing Hive environment:

Only Presto compute nodes must be deployed:
1. Configure Presto to use the existing Hive metastore
   - No schema migration is necessary
   - Any schema changes initiated by either Hive or Presto are immediately reflected in both systems
2. Existing HDFS files may be reused
   - No data migration is required
   - No file format changes are required:
     - Files are self-describing with no metadata to sync elsewhere

This ease-of-adoption is incredibly attractive for new open-source projects.
- [Data has gravity.](/training/qram/raw/definition_of_data_gravity) Copies and migrations are incredibly expensive in machine terms, and no physical migration of data is necessary.
- Zero-risk of semantic and syntactic drift (schema). The database schema stays in lock-step across all query engines.

# Cons

## Hive Metastore
- "One location per partition" prevents append-only data structures, a key performance optimization.
- Lack of referential transparency prevents warehouse cloning and time travel.
- Self-describing files avoiding metadata cache incoherence issues leads to massive stability, scalability, and performance issues.
  - Needing to hit a file system to get file metadata is a major drag.
  - [This article by Cloudera simply says, "don't do that!"](https://blog.cloudera.com/partition-management-in-hadoop/)
  - Impala chose to cache metadata on every node. This over-correction then leads to massive instability at large scale.
    - [In Impala, since v2.10, the HDFS NameNode may be cached.](https://impala.apache.org/docs/build3x/html/topics/impala_scalability.html)
    - This cache exists on every Impala node. It is enabled by default at runtime for a given Impala node once that node sees >= 20,000 HDFS file names. Caching may be turned on completely. The 20,000 file name threshhold may be adjusted as well. There is no way to completely disable caching.

(This caching is not "HDFS caching", which is caching file contents from HDFS DataNodes.)

# References
- [Partition Management in Hadoop (by Cloudera)](https://blog.cloudera.com/partition-management-in-hadoop/)
- [Impala HDFS NameNode Cache](https://impala.apache.org/docs/build3x/html/topics/impala_scalability.html)
