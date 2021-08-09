---
title: Determining the number of Mappers and Reducers in a Hive Query
description: 
published: true
date: 2021-07-30T00:21:41.081Z
tags: hive, map-reduce
editor: markdown
dateCreated: 2021-07-30T00:21:41.081Z
---

The maximum number of mappers for any given query depends on the number of Hive splits for the input data. The Hive metastore allows a set of individual files and directories to be the "location" of a Hive table. Default Hive configuration gives Hive total control over its backing filesystem, which is 99% of the time HDFS. In the default configuration, each Hive single-partition-table and Hive table-partition-that-is-part-of-a-multi-partition-table is backed by all of the files located inside of the HDFS directory set as the "location" property of the Hive schema object (single-partition-table or the other thing). If you decide to manually control the backing filesystem, each stated partition can manually point to a file or directory in the backing filesystem. (Manually controlling the backing filesystem is necessary if multiple compute engines are using the files, such as a dual-Hive-and-Presto environment.

Each file on HDFS is stored as blocks. HDFS is an application-level FS implemented in Java, as opposed to a regular FS that is part of the OS kernel. HDFS provides a veneer on top of the actual OS FS. In this veneer, HDFS has a configurable "block size", by default 128Mb. Every file is stored as one or more blocks. So a 5Mb file occupies a full 128Mb HDFS block. (As a side note, that HDFS block is virtual; asking HDFS how much space it is using will say 128Mb even when the underlying OS kernel FS stores the 5Mb as, let's say, 5.1Mb (some multiple of the OS kernel FS block size).
So in Hive on HDFS, every table partition is at least one file, and each one of those files is at least one block on HDFS.

---

Here's where it gets stupid

Hive has the concept of a "split size" which is the same as HDFS "block size". Because Hive can sit in front of other FS's too, it can't assume that HDFS is the backing FS, so it has its own "block" type concept. And the "split size" in Hive is 128Mb by default too. **If Hive is using HDFS, and the split size is smaller than the HDFS block size, the split size is silently overridden by the HDFS block size**

Now, that's the maximum number of mappers. The actual number of mappers per query can then be capped by the on-startup configuration of Hive in some property. This may then be overridden by the actual engine behind Hive. Most Hive today uses Tez, so Tez configuration actually wins, but older installations use classic MR, in which case classic MR configuration wins

Whatever that on-startup configuration is, if there's some resource governor controlling Hive, it may further reduce the number of mappers to ensure that queries received later are not resource starved. Anyone paying for a Cloudera license for open source Apache Hadoop will be using the Cloudera resource governor

If they ask you this, just walking into the mapper count will soak up the interview time, I'm sure. You probably can punt on reducers. And IDK but I expect that very few candidates actually have an answer, so simply breaking out "well, it depends on the Hive split size, HDFS block size (if the backing FS is HDFS, Hive configured max mappers, if there's a resource governor, and the actual block layout of the files for every Hive table partition read in the specific SQL statement" is probably enough. This is probably just a, "has this person ever tuned Hadoop ecosystem before" checkbox.

## References

* https://www.fatalerrors.org/a/how-does-hive-determine-the-number-of-map-s-and-reduce.html'
* https://community.cloudera.com/t5/Community-Articles/How-Hive-determines-the-number-of-splits/ta-p/246467
* https://medium.com/airbnb-engineering/on-spark-hive-and-small-files-an-in-depth-look-at-spark-partitioning-strategies-a9a364f908
