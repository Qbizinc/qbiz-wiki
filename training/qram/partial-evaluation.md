---
title: Partial Evaluation
description: 
published: true
date: 2021-07-30T00:14:48.164Z
tags: aggregation, horizontal-scalability
editor: markdown
dateCreated: 2021-07-30T00:14:48.164Z
---

## What?

Incremental evaluation where partial values are serializable.

If deserializing the value requires as much resource (usually wall time latency or compute) as evaluating from inputs, then it's not partial evaluation!
* By definition, we are horse trading between resources to optimize for some chosen cost function
* Of course, the better the trade, the better the partial evaluation algorithm
-- This is always use case dependent

## Why?

Serializability allows for I/O: durable storage or network.

Examples:
* Cache the partial value to avoid re-evaluation later
-- Materialized view, aggregate table, any downstream ETL table where the upstream data is not destroyed (i.e. this is actually a cache, not a lossy compression).
* Output the serialization of the partial value to another node in a compute cluster
-- Scaling across network (one of the requirements of horizontal scalability in current practice)

## Related
* [Horizontal Scalabilty](/training/qram/horizontal-scaling)
* Incremental evaluation
* [Additivity](/training/qram/additivity)
