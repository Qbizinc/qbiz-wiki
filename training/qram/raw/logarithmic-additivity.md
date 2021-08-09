---
title: Logarithmic Additivity for Ordered Collection Slices
description: 
published: true
date: 2021-07-29T19:24:54.146Z
tags: aggregation, partitioning
editor: markdown
dateCreated: 2021-07-29T19:23:32.700Z
---

## What
Aggregating slices of ordered collections is a common operation. The most common example is `SELECT aggregate() FROM t WHERE dt >= start and dt < end`. dt is ordered, and the start and end form a 1-dimensional slice.
Using additivity in a logarithmic way allows read time to reduce number of rows read from linear to logarithmic, at a cost of increasing storage by a constant factor <2 (storage is never more than doubled).

## Example

1. Create an aggregate table keyed by date interval.
1. Take every two-day pair and compute the aggregate, storing it in the table.
1. Repeat for every four-day set, eight-day set, recursively. **Store these (INSERT) in the same table.** (This is a logarithm base-2 example.)
1. On read, the slice expression (WHERE dt >= start and dt < end) has to be rewritten into the set of 1-day, 2-day, 4-day, 8-day, etc. slices that add up to the date range.
> This rewrite is context-free, i.e. it can be done without reading any data from disk, so it can be implemented on something like Hive or Presto with predicate-pushdown partition pruning.

> Yes, this is math, but it's not so bad.

5. The slice expression resolves to just grab those specific records.
6. Rows read is log-base-2; storage is doubled.
## Related
﻿Additivity