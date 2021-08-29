---
title: Definition of Hive Metastore
description: 
published: true
date: 2021-08-29T21:57:18.407Z
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

- For tables with "too many" partitions, HMS (usually PostgreSQL) falls down. ()
- For tables with too many files belonging to the location of any given partition, the HDFS NameNode falls down.

Therefore:
- `[# of files / table] = [# of partitions / table] * [# of files / partition]`
- `[optimal # of files / table] = [table size in bytes] / [low integer multiple of HDFS block size in bytes]`
- Shifting the balance requires:
  - Moving partitions on HMS to adjust `[# of partitions / table]`
  - Relocating the files on the HDFS NameNode to make partition locations work (every file in a partition must be in a subdirectory tree of the partition location)
- Both the HMS and the HDFS NameNode are single-instance by design.
