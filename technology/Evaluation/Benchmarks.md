---
title: Benchmarks
description: Benchmark resources
published: true
date: 2023-10-11T21:31:27.574Z
tags: 
editor: markdown
dateCreated: 2023-10-11T21:19:04.281Z
---

# Benchmarks
## TPC Benchmarks

The TPC is a consortium of companies and institutions that have been defining database benchmarks since 1985.  It has lost much of its cachet since its heights in the late 1990s through the early 2000s, probably due to vendor consolidation and the rise of open source solutions – which dramatically changed the economic of purchasing a database solution.  It remains relevant insomuch as there are no other independent tools by which to measure the ever evolving data solution market.

As the various benchmarks lose favor, their tools and documentation become outdated.  For example, the TPC-DS benchmark's tools are writer in C and must be compiled from source and its documentation has many errors, such as incorrectly mentioning the query generation tool as qgen2, which is the application's name in a previous version – the true name in TPC-DS v3.2 is dsqgen.  I had to read several Stack Overflow entries to resolve compilation issues (must use gcc-9) and runtime issues ([see resolution](https://dba.stackexchange.com/questions/36938/how-to-generate-tpc-ds-query-for-sql-server-from-templates/97926#97926) to *ERROR: Substitution'_END'*)

All the benchmarks have two tools: one for generating data; the other  for generating queries.  The query generator builds a query suite for various database vendors, such as Oracle, db2, Netezza, etc...  There is also an ANSI flavor.

### TPC-DS

The Decision Support is the classic benchmark for Data Warehousing solutions.  Recently, this benchmark was used by [Databricks in 2021](https://www.databricks.com/blog/2021/11/02/databricks-sets-official-data-warehousing-performance-record.html) to showcase the power of its platform and prove that their technology can be used for Data Warehouse Use Cases.  The data used in this benchmark can be found in [Snowflake's Public Schema](https://docs.snowflake.com/en/user-guide/sample-data-tpcds) and has also run their own benchmark and [blog](https://www.snowflake.com/blog/industry-benchmarks-and-competing-with-integrity/).

The benchmark is composed of 99 queries – see the Snowflake Blog for a summary – designed to mimic DW workloads and use cases.

I was able to get query suite with the following CLI: `./dsqgen -DIRECTORY ../query_templates -INPUT ../query_templates/templates.lst -VERBOSE Y -SCALE 10 -DIALECT ansi -OUTPUT_DIR /tmp`

### TPC-H

The Hadoop benchmark was created to compare various Hadoop vendors, such as Cloudera or Hortonworks (which merged by Cloudera in 2018).  Although not so much cited as the the TPC-DS benchmark, it is still used to prove Hadoop workloads perform fine on platforms like Databricks and Snowflake.

### TPCx-AI

The TPCx-AI is an updated benchmark for Artificial intelligence.  