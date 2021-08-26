---
title: Tez Architecture
description: 
published: true
date: 2021-08-26T18:35:14.451Z
tags: 
editor: markdown
dateCreated: 2021-08-26T18:35:14.451Z
---

# Tez Architecture

Like Spark, Apache Tez is an open-source framework for big data processing based on the MapReduce technology. Both Spark and Tez offer an execution engine that is capable of using directed acyclic graphs (DAGs) to process extremely large quantities of data.

Tez allows tools like Hive to define their overall processing steps using DAGs before the job begins. Because the entire steps can be computed before execution time, the system can take advantage of caching intermediate job results in memory, reducing the number of disk access that Map Reduce has.

Another advanatge is that the Tez execution engine framework efficiently acquires resources from YARN and reuses every component in the pipeline such that no operation is duplicated unnecessarily.

## Tez Components

Tez is a framework that is build on top of Yarn. It uses Yarn for cluster Management and resource allocation:

- **Tez AppMaster** processes the Job Plan by calling the Yarn Resource Manager to allocate the task containers. The Tez AppMaster takes full responsibility to use the containers to implement an effective job runtime. The TezAppMaster is responsible for dealing with transient container execution failures and must respond to Resource Manager requests regarding allocated and possibly deallocated Containers. It is a single point of failure for a given job.

- **Node Manager** executes the worker processes within the allocated *Tez containers*

Tez also has two main API's:

- *DAG API*: Defines the Structure of the Data Flow. Defines Complex Data Flow pipelines using simple graph DAG APIs

- *Runtime API* Transforms the Data and used to specify what actually executes in each task

## DAG API

- DAG: This defines the overall job. The user creates a DAG object for each data processing job.
- Vertex: This defines the user logic and the resources & environment needed to execute the user logic. The user creates a Vertex object for each step in the job and adds it to the DAG.
- Edge: This defines the connection between producer and consumer vertices. The user creates an Edge object and connects the producer and consumer vertices using it.

## How to setup Hive with Tez

Reference: https://docs.cloudera.com/HDPDocuments/HDP2/HDP-2.6.4/bk_command-line-installation/content/ref-c8619e80-1c00-4863-a4e8-ad79a58c178f.1.html

# References
- [Tez Documentation](https://tez.apache.org/)
- [Cloudera Apache Tez](https://www.cloudera.com/products/open-source/apache-hadoop/apache-tez.html)
- [Hadoop Summit Presentation](https://www.slideshare.net/Hadoop_Summit/apache-tez-present-and-future)


