---
title: HDFS Limitations
description: 
published: true
date: 2021-08-11T02:43:22.122Z
tags: 
editor: markdown
dateCreated: 2021-08-11T02:43:22.122Z
---

Newer distributed separate-storage-and-compute query engines are often designed and architected to support HDFS legacy architecture. This includes:
- Presto
- Impala
- Spark

(Not Snowflake, which bit the bullet and uses its own proprietary storage.)

NameNode

DataNode (which node to retrieve from if replication factor)
