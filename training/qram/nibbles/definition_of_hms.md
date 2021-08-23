---
title: Definition of Hive Metastore
description: 
published: true
date: 2021-08-23T20:22:48.319Z
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

## Architecture
HMS is a single-instance Java process using the DataNucleus ORM to read and write from a relational database, usually PostgreSQL.

# High Availability
It is possible to deploy the Hive Metastore in High Availability mode (HA).

However, this is not in the Apache distributions. Each vendor has their own implementation of HMS HA.

In general, HA for HMS means a single active HMS node with multiple standby nodes. Failure of the active leads to a standby being chosen to become the active.

Usually, all of the HMS nodes read and write from the same relational database network address and port.

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
- `[# of files / table] = [# of partitions / table] * [# of files / partition]`
- `[optimal # of files / table] = [table size in bytes] / [low integer multiple of HDFS block size in bytes]`
- Shifting the balance requires:
  - Moving partitions on HMS to adjust `[# of partitions / table]`
  - Relocating the files on the HDFS NameNode to make partition locations work (every file in a partition must be in a subdirectory tree of the partition location)
- Both the HMS and the HDFS NameNode are single-instance by design.

### Impala
[In Impala, since v2.10, the HDFS NameNode may be cached.](https://impala.apache.org/docs/build3x/html/topics/impala_scalability.html) This seeks to avoid the HDFS NameNode side of this stability issue, as well as dramatically improve performance by avoiding reading from the HDFS NameNode.

This cache exists on every Impala node. It is enabled by default at runtime for a given Impala node once that node sees >= 20,000 HDFS file names. Caching may be turned on completely. The 20,000 file name threshhold may be adjusted as well. There is no way to completely disable caching.

(This caching is not "HDFS caching", which is caching file contents from HDFS DataNodes.)

- In large installations, HDFS NameNode caching has been known to bring down clusters, or at least make startup times so long that DevOps is impractical.
- The cache may lead to weird inconsistency errors, as Impala nodes must communicate changes (writes) to schema/table/partition/location to each other on a peer-to-peer basis. The probability of this increases with the number of partitions in an installation.

# References
- [Hive Design](https://cwiki.apache.org/confluence/display/hive/design)
- [Partition Management in Hadoop (by Cloudera)](https://blog.cloudera.com/partition-management-in-hadoop/)
- [Impala HDFS NameNode Cache](https://impala.apache.org/docs/build3x/html/topics/impala_scalability.html)
