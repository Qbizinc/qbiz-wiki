---
title: Definition of Additivity
description: 
published: true
date: 2021-08-10T18:09:38.030Z
tags: aggregation, horizontal-scalability
editor: markdown
dateCreated: 2021-07-29T19:47:51.148Z
---

# What
One way to allow for the partial evaluation of aggregation

## Trivial example:
1. A table with individual sales transactions which has a revenue field is input into an evaluation: return the sum of revenue.
2. A downstream table storing the aggregation of these sales transactions by time periods (calendar months) may be used as the input instead.
3. This downstream table is contains the partial evaluation of total revenue for all time
4. Revenue at the calendar month level is additive toward total revenue
   * The individual calendar month revenues may be further aggregated to the total level
   * **NB:** this does not count as partial evaluation if using the higher level pieces isn't lower cost (per the chosen cost function) than simply using the lower level inputs

## Basic example:
* Average (arithmetic mean) is not additive in and of itself (average of sets may not be further aggregated into the average of any supersets).
* However, average may be stored as sum and count, and any evaluation of average may instead aggregate those partial values into values at the desired level, and do a single scalar operation at that level, e.g. division, to calculate the average at that level.
* This also allows for distributed compute. See [additive of arithmetic mean](/training/qram/nibbles/additive_of_arithmetic_mean).

## Formal What
An aggregation operation is additive if and only if:
* The operation may be expressed as a scalar operation of serializable pieces.
* These pieces are themselves the result of an aggregation operation that is commutative and associative.

Aggregation operations that are commutative and associative are also additive, because the scalar operation is a no-op (formally, an identity operation).

# Why?

For the savings!

This is especially powerful in data lakes (Hadoop-style separate storage and compute) with incremental ETL, i.e. keep adding 1-day partitions to a multi-partition table.
* Save the commutative and associative pieces of the measure to allow for multi-day aggregations without going back in time to recalculate.
* Again, this requires the SQL reading the table to include the scalar operation.

# Note

Sum and count are examples of aggregation operations that are commutative and associative.

The machine doing the last step aggregation must be capable of evaluating the scalar operator.

* In SQL, using materialized aggregates requires the user writing the final SQL expression to use the scalar operator at the end, e.g. the user must be educated
* Data engines with materialized view SQL rewriting can be very powerful if the final scalar operator can be made known to the data engine. (No one actually does this yet.)
* In BI platforms, if attempting to use materialized aggregates (relational or not), the scalar operator must be expressible in the BI platform itself.
  > Yay Looker! Boo Tableau. (LookML allows for scalar operations to be injected in the measure definition SQL. Tableau is inflexible.)
* Incremental training of machine learning models is possible if each incremental operation is additive. See this phat WIP.
