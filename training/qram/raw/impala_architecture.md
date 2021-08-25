---
title: Impala Architecture
description: 
published: true
date: 2021-08-25T17:18:32.419Z
tags: 
editor: markdown
dateCreated: 2021-08-25T17:18:32.419Z
---

# Impala Architecture
The Impala server is a distributed massively parallel processing (MPP) database engine. Similar to Hive, with Impala you can query data in HDFS in real time. Furthermore, Impala uses the same metadata, SQL syntax, ODBC driver, and user interface as Apache Hive.


## Hive vs Impala

Hive was developed to provide a SQL read query interface to Hadoop MapReduce. MapReduce is not used for parallel computing. Although MapReduce is a very good parallel computing framework, it is more batch-oriented rather than an interactive SQL execution. Compared with MapReduce Impala divides the entire query into an execution plan tree (daemons) instead of a series of MapReduce tasks.

## Components

There are three mayor components of Impala's Architecture:

- Imapala Daemon
- Impala Statestore
- Impala Catalog Service

## Impala Daemon (Impalad)

The core Impala component is a daemon process that runs on each DataNode of the cluster, physically represented by the impalad process. It reads and writes to data files; accepts queries transmitted from the impala-shell command, Hue, JDBC, or ODBC; parallelizes the queries and distributes work across the cluster; and transmits intermediate query results back to the central coordinator node.


## Impala Statestore

The Impala component known as the statestore checks on the health of Impala daemons on all the DataNodes in a cluster, and continuously relays its findings to each of those daemons. If an Impala daemon goes offline due to hardware failure, network error, software issue, or other reason, the statestore informs all the other Impala daemons so that future queries can avoid making requests to the unreachable node.

## Impala Catalog Service

Impala uses traditional MySQL or PostgreSQL databases to store table definitions. The important details such as table & column information & table definitions are stored in a centralized database known as a metastore.

# References
- [Impala Documentation](https://docs.cloudera.com/documentation/enterprise/6/6.3/topics/impala_components.html)
- [Configure Impala with Hive](https://docs.cloudera.com/runtime/7.2.8/tuning-hue/topics/hue-configure-hive-impala-for-ha-with-hue.html)