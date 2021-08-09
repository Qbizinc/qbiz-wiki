---
title: Additive of Distinct Count (Approx)
description: 
published: true
date: 2021-08-09T21:30:17.936Z
tags: aggregation, horizontal-scalability
editor: markdown
dateCreated: 2021-07-29T21:05:55.532Z
---

# What

Distinct count (cardinality of a multiset) is not additive.

An approximation may be expressed using [HyperLogLog](https://en.wikipedia.org/wiki/HyperLogLog).

The digest is a data structure which is a serialization of an aggregate operator that is commutative and associative.  This data structure is called a sketch.

A really complex scalar operator is required at the end to extract from the final aggregate data structure. The final scalar operator may specify the desired accuracy, so long as the aggregate operator preserves that accuracy, at the cost of aggregator compute and serialization size.

# Notes

HLL uses random sampling to form a crappy estimator, then combining with harmonic mean.

In Presto:
> 1. approx_set(v) -> HLL sketch
> 2. merge(sketch) = aggregation of HLL sketches
>    This is the aggregation operator.
> 3. cardinality(sketch) = final distinct count
>    This is the scalar operator.
{.is-info}
