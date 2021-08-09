---
title: Additive of Population Variance and Standard Deviation
description: 
published: true
date: 2021-08-09T23:35:28.241Z
tags: aggregation, horizontal-scalability
editor: markdown
dateCreated: 2021-07-29T20:58:34.117Z
---

# What

## Definitions
![variance.png](/variance.png)

### [Variance](https://en.wikipedia.org/wiki/Variance)
measures the average degree to which each point differs from the [mean](https://en.wikipedia.org/wiki/Arithmetic_mean), detailed in the equation above.

This can be broken down into associative/commutative pieces  
* The Σ is analogous to SUM(column)
* The N is analogous to COUNT(column)

### [Standard deviation](https://en.wikipedia.org/wiki/Standard_deviation)
measures the how spread the group of numbers is from the mean, by taking the square root of variance.

# Algorithm

```
# data = some_data 
n = 0 # COUNT(column)
partial_sum = 0 # SUM(column)
partial_sum_squared = 0 # SUM(column) * SUM(column)

for x in data: # for each point in data
    n += 1
    partial_sum += x
    partial_sum_squared += x * x
    
variance = (partial_sum_squared - (partial_sum * partial sum) / n) / n 
```

# Example

![standard_deviation.png](/standard_deviation.png)

# Additive Formula

variance = (sum(v^2) - sum(v)^2/count(v)) / count(v)
stddev = sqrt(variance) [an extra scalar operation]
sum and count are commutative and associative
