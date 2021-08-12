---
title: Definition of Hive Metastore
description: 
published: true
date: 2021-08-12T21:53:10.604Z
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

### Location Property Limitations
That each partition may only have a single location is a major limitation to Hive architecture. In contrast, Snowflake and Delta Lake internally allow multiple paths, which sets the foundation for:
- Persistent data structures (PDS)
  - Avoids full-copy-on-write
- Serializable isolation-level version control
  - Clean history preservation for auditing and data lineage
  - Time travel

## Implementation
Usually, HMS is hosted on a single-node PostgreSQL database. This has fault tolerance implications.

## Performance
Because the Hive Metastore is the only metadata store for query engines which use it, querying it to get the HDFS paths to use for a query always has to be at the table level. For tables with many partitions (FAANG-scale may be hundreds of thousands of partitions per table), PostgreSQL falls down. ([This annoying article by Cloudera simply says, "don't do that!"](https://blog.cloudera.com/partition-management-in-hadoop/)) The real issue is that any query engine that uses **WHERE-clause-predicate-pushdown-into-FROM-clause** cannot send the Hive Metastore a query that filters on those **FROM-clause-predicates** because the query engine cannot know ahead of time what the location property may contain or even the order of the predicates for the directory structure. Therefore, a SQL query that the query engine knows ahead of time will hit only one partition still has to get all of the partitions for the corresponding table from HMS.

## Other Query Engines
- Presto
- Impala
- Spark (though Databricks now encourages Delta Lake)

# References
- [Hive Design](https://cwiki.apache.org/confluence/display/hive/design)
- [Partition Management in Hadoop (by Cloudera)](https://blog.cloudera.com/partition-management-in-hadoop/)
