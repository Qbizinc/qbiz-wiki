---
title: Additive of Percentile (Approx)
description: 
published: true
date: 2021-07-29T23:37:14.032Z
tags: aggregation, horizontal-scalability
editor: markdown
dateCreated: 2021-07-29T23:37:14.032Z
---

See [additivity](/training/qram/additivity)

## What

Percentile, quartile, and median are not additive.

An approximation may be expressed using [t-digest](https://github.com/tdunning/t-digest) and [Q-digest](http://www.inf.fu-berlin.de/lehre/WS11/Wireless/papers/AgrQdigest.pdf).

Both of these data structures are serializations of aggregate operators that are commutative and associative.

A really complex scalar operator is required at the end to extract from the final aggregate data structure.

In both cases, the final scalar operator may specify the desired accuracy, so long as the aggregate operator preserves that accuracy, at the cost of aggregator compute and serialization size.

## Note

t-digest uses 1-dimensional k-means clustering under the hood.

In [Presto](https://prestodb.io/docs/current/functions/qdigest.html):
* qdigest_agg(v) -> qdigest type blob
* merge(blob) = aggregation of qdigest type blobs
* value_at_quantile(blob, v) = final percentile of v

[This approach is not widely known](https://stackoverflow.com/questions/2571358/calculate-the-median-of-a-billion-numbers)