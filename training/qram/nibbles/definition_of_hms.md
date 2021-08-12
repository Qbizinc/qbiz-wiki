---
title: Definition of Hive Metastore
description: 
published: true
date: 2021-08-12T23:12:50.510Z
tags: 
editor: markdown
dateCreated: 2021-08-11T02:52:21.513Z
---

# What
The Hive Metastore (HMS) stores the relational database metadata for Hive:
- Schema and tables
- For each table, its partitions (if multi-partition table)

## Location Property
Each single-partition table and partition (in multi-partition tables) has a location property, that contains a file system path. This path may be a file or a directory. The Hive Metastore simply stores this as a string; it's up to Hive (or whichever query engine) to decipher the path.

## Implementation
Usually, HMS is hosted on a single-node PostgreSQL database. This has fault tolerance implications.

# Implications

## Location Property
That each partition may only have a single location is a major limitation to Hive architecture. In contrast, Snowflake and Delta Lake internally allow multiple locations, which sets the foundation for:
- Persistent data structures (PDS)
  - Avoids full-copy-on-write
- Serializable isolation-level version control
  - Clean history preservation for auditing and data lineage
  - Time travel

## Stability and Performance
HMS stores locations at the partition-level. Query engines must then read the list of files for each partition location from the HDFS NameNode.

- For tables with "too many" partitions, HMS (usually PostgreSQL) falls down. ([This annoying article by Cloudera simply says, "don't do that!"](https://blog.cloudera.com/partition-management-in-hadoop/))
- For tables with too many files belonging to the location of any given partition, the HDFS NameNode falls down.

Therefore:
- `# of files in a table = # of partitions in a table * # of files per partition`
- `optimal # of files in a table = low integer multiple of HDFS block size`
- Shifting the balance requires:
  - Moving partitions on HMS to adjust `# of partitions in a table`
  - Relocating the files on the HDFS NameNode to make partition locations work (every file in a partition must be in a subdirectory tree of the partition location)
- Both the HMS and the HDFS NameNode are single-instance by design.

# References
- [Hive Design](https://cwiki.apache.org/confluence/display/hive/design)
- [Partition Management in Hadoop (by Cloudera)](https://blog.cloudera.com/partition-management-in-hadoop/)
