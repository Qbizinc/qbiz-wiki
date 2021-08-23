---
title: Definition of Hive Type
description: 
published: true
date: 2021-08-23T18:25:45.173Z
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

# Adoption
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

# Prevents Innovation
Snowflake envy:
- "One location per partition" prevents append-only data structures, a key performance optimization in Snowflake.
- HMS generally prevents data warehouse cloning and time travel, also in Snowflake.
- Self-describing files avoiding metadata cache incoherence issues leads to massive stability, scalability, and performance issues. (Also solved in Snowflake.)
