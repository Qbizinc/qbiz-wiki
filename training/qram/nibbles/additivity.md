---
title: Additivity
description: 
published: true
date: 2021-08-10T18:21:29.082Z
tags: 
editor: markdown
dateCreated: 2021-08-10T18:21:29.082Z
---

# What
A technique for optimizing performance of aggregate operations at read-query time:
* Distributed compute engines can shuffle less, reducing network utilization and read latency.
* Partial evaluation of the aggregate operation may be written and saved to disk in a way that minimizes the amount of data read to compute the aggregate.

## Approximations
Certain aggregate operations are not additive and cannot be optimized. The classic example is ***distinct count***. ***n-tiles*** are also included. These operations are known to be extremely compute-intensive.

However, there are ways to approximate these with well-known error bars in an additive way.
* The additive pieces are typically some opaque, binary-format *digest*.
* These *digests* may be combined with each other.
* These *digests* have to go through a scalar operation to extract the usable value from the *digest*.

See ***distinct count*** and ***n-tile*** below.

# Nibbles
1. [Definition of Additivity](/training/qram/nibbles/definition_of_additivity)
2. [Additive of Sum](/training/qram/nibbles/additive_of_sum)
3. [Additive of Average (Arithmetic Mean)](/training/qram/nibbles/additive_of_arithmetic_mean)
4. [Additive of Population Variance and Standard Deviation](/training/qram/nibbles/additive_of_population_variance_and_standard_deviation)
5. [Additive of Distinct Count (Approx)](/training/qram/nibbles/additive_of_distinct_count_approx)
6. [Additive of N-Tile (Approx)](/training/qram/nibbles/additive_of_n_tile_approx)
