---
title: Hive Architecture
description: 
published: true
date: 2021-08-22T06:45:07.053Z
tags: 
editor: markdown
dateCreated: 2021-08-19T17:58:44.857Z
---

# Hive Type Architecture
Hadoop MapReduce was the first open-source Big Data query engine. It spawned a new open-source, standards-based architecture for distributed, separate-storage-and-compute read query engines.

Hive was developed later under the Hadoop umbrella to provide a SQL read query interface to Hadoop MapReduce.

Because of the open-source, standards-based nature of Hive, newer SQL-based query engines are designed to be compatible with this architecture.

Qbiz categorizes this type of architecture as "Hive-type". (The term "Hive-type" is not in common usage.)

There are four main components in the Hive architecture:

1. HDFS and MapReduce
2. Metastore
3. Driver
4. Hive Clients

## HDFS and MapReduce

