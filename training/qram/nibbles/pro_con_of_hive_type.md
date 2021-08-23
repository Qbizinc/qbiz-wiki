---
title: Pros and Cons of Hive Type
description: 
published: true
date: 2021-08-23T20:01:13.433Z
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

## Slows Innovation
Snowflake envy:
- "One location per partition" prevents append-only data structures, a key performance optimization in Snowflake.
- HMS generally prevents data warehouse cloning and time travel, also in Snowflake.
- Self-describing files avoiding metadata cache incoherence issues leads to massive stability, scalability, and performance issues. (Also solved in Snowflake.)
  - Needing to hit a file system to get file metadata is a major drag.
  - It's so bad that Impala created an HDFS NameNode cache on each Impala node. This cache then leads to massive instability at large scale.
