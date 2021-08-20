---
title: Definition of Slice Oriented File Format
description: 
published: true
date: 2021-08-20T16:26:26.954Z
tags: 
editor: markdown
dateCreated: 2021-08-11T22:26:45.011Z
---

# What
These are file formats optimized to take advantage of file systems that allow reading byte-offset slices of files, i.e. as in HDFS.

In Big Data, where files or even blocks of files may be quite large, the ability to only get slices is critical. This is only useful if the file format actually takes advantage of this feature.

For an example of a file format where this doesn't work:
1. A file format that is compressed (i.e. with gzip)
2. Retrieve 200 bytes starting at byte number 100 (0-based)
3. The very first byte of this block is in the middle of some multi-byte control character in gzip
4. There is no way to make sense of this slice

## Influence of Hadoop
Much of the work in this space came out of Hadoop, which originally used a format named "Sequence File". The ability of the file system to provide byte-offset slices is a cornerstone of Hadoop-type and [Hive-type](/training/qram/nibbles/definition_of_hive_type) architecture.

# How

## Common

### File Format Header and Footer
These denote the file format, but also denote that the file is actually complete, which avoids readers processing files that aren't finished being written.

Typically, these are the first and last 4-bytes in utf-8. The slice feature of HDFS allows readers to ask for these first to confirm:
- the file is well-formed
- the file format itself

For example, Parquet has `PAR1` in utf-8 (same as ascii) as the first and last 4-bytes.

The exception is Hadoop Sequence Files, which only has a header of `SEQ<#>`, where `<#>` is the format version number. The latest version is `6`.

### Block-Level Compression
In the above example, gzip was shown not to be compatible with arbitrary slicing. (Mathematically, compression is identical to encryption, so blobs are only readable if the decompression algorithm has the whole blob.)

The solution is to simply split up the file into blocks and compress those independently. Each block has to be decompressed separately, but at least the entire file isn't locked away. Some compression-factor is lost, but the ability to slice more than makes up for it.

**NB:** "Block" here is not the same as "file system block". It merely means the logical act of splitting up the file on some data structure boundary.
- Row-oriented: some number of rows
- Column-oriented: a single column of data for some number of rows

## Row-Oriented
Both Hadoop Sequence Files and Avro use block-level compression with sync markers between each block. The blocks contain some number of rows, one complete row at a time. Each individual row adheres to the specifics of the format.

**NB:** Hadoop Sequence Files do not have to use block compression. This article is written assuming block compression, as it is most common. Record-level compression and uncompressed are available as well; uncompressed is most common for streaming ingestion with batch downstream ETL.

### Sync Markers
- The sync markers let the reader process know where blocks begin and end. (This implies that the sync markers don't look like anything inside any of the compressed blocks.)
- The sync markers also point to the next (or previous) sync marker by byte-position, letting the reader know exactly how to skip around once a single sync marker has been located.
- The sync markers are not inside of the compressed blocks, which allows the reader process to access them.

For example:
1. A single 256 MB file is read by Hadoop MapReduce.
2. The Hadoop installation has a Sequence File block size set to 16 MB in `core-site.xml`.
3. There are 8 mappers, which divide into the 256 MB to give each 32 MB.
4. The first 7 mappers read 48 MB (32 MB + 16 MB block size), and the last mapper reads the last 32 MB.
   - This takes advantage of HDFS's ability to read by byte-offset slice.
   - The extra 16 MB is to guarantee that no blocks are unprocessed (see #5).
5. Each mapper finds the very first sync marker and reads until the first sync marker ***on or after*** the 32 MB boundary.
   - This ensures no blocks are lost.
   - This also ensures no blocks are processed more than once.

## Column-Oriented
Both Parquet and ORC have footers at the end of each file that provide the byte-offsets and lengths of each:
- number of records (i.e. every 2^16 records)
- for each one of those record blocks, every column

This nested data structure is summarized in the file metadata in the footer.

Every file:
1. has a file format header and footer (first and last 4-bytes)
2. length of file metadata in bytes (last 4-bytes before footer); standards have converged on little-endian unsigned int
3. file metadata (before length of file metadata 4-bytes)
4. file contents (everything between header and file metadata)

To use column-oriented file formats, readers:
1. get the header and footer to ensure well-formed and to get the format
2. get the length of file metadata
3. get the file metadata
4. determine which record blocks are desired
   - the file metadata contains predicates about the contents, letting query engines know whether a record block may be skipped
5. for each desired record block, get the desired columns

All of this requires a file system that supports reading slices.

# References
- [HADOOP2 Sequence File](https://cwiki.apache.org/confluence/display/HADOOP2/SequenceFile)
- [Avro, Parquet, ORC by Toward Data Science](https://towardsdatascience.com/demystify-hadoop-data-formats-avro-orc-and-parquet-e428709cf3bb)
- [by Clairvoyant Blog](https://blog.clairvoyantsoft.com/big-data-file-formats-3fb659903271)
- [Avro FAQ: Sync Marker](https://cwiki.apache.org/confluence/display/AVRO/FAQ#FAQ-Whatisthepurposeofthesyncmarkerintheobjectfileformat)
