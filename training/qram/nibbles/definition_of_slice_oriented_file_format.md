---
title: Definition of Slice Oriented File Format
description: 
published: true
date: 2021-08-11T22:26:45.011Z
tags: 
editor: markdown
dateCreated: 2021-08-11T22:26:45.011Z
---

# What
These are file formats optimized to take advantage of file systems that allow reading byte-offset slices of files, i.e. as in HDFS.

In Big Data, where files or even blocks of files may be quite large, the ability to only get slices is critical. This is only useful if the file format actually takes advantage of this feature.

For an example of a file format where this doesn't work:
> 1. A file format that is compressed (i.e. with gzip)
> 2. Retrieve 200 bytes starting at byte number 100 (0-based)
> 3. The very first byte of this block is in the middle of some multi-byte control character in gzip
> 4. There is no way to make sense of this slice

## Influence of Hadoop
Much of the work in this space came out of Hadoop, which originally used a format named "Sequence File". The ability of the file system to provide byte-offset slices is a cornerstone of Hadoop-type and Hive-type architecture.

# How

## Common

### Block-Level Compression
In the above example, gzip was shown not to be compatible with arbitrary slicing. (Mathematically, compression is identical to encryption, so compressed blobs are unreadable without the right keys and partial blobs are completely unreadable.)

The solution is to simply split up the file into blocks and compress those independently. Some compression-factor is lost, but the ability to slice more than makes up for it.

**NB:** "Block" here is not the same as "file system block". It merely means the logical act of splitting up the file on some data structure boundary.

## Row-Oriented
Both Hadoop Sequence Files and Avro use block-level compression with sync markers between each block. The sync markers are not inside of the compressed blocks

For example:
> 1. Compressed 

## Column-Oriented

# References
- [HADOOP2 Sequence File](https://cwiki.apache.org/confluence/display/HADOOP2/SequenceFile)
- [Avro, Parquet, ORC by Toward Data Science](https://towardsdatascience.com/demystify-hadoop-data-formats-avro-orc-and-parquet-e428709cf3bb)
- [by Clairvoyant Blog](https://blog.clairvoyantsoft.com/big-data-file-formats-3fb659903271)
- [Avro FAQ: Sync Marker](https://cwiki.apache.org/confluence/display/AVRO/FAQ#FAQ-Whatisthepurposeofthesyncmarkerintheobjectfileformat)
