---
title: Merging Intervals
description: 
published: true
date: 2022-02-15T20:41:05.937Z
tags: interview_question
editor: markdown
dateCreated: 2022-02-15T19:56:10.429Z
---

# Interview Question: Merging Intervals
>**Language: Python3**
{.is-info}

>**Difficulty: Medium**
{.is-warning}

## Question Text

```python
# Given a list of interval tuples 
# Create a function to merge overlapping and adjacent intervals 
# intervals = [ (1,3), (9,17), (4,7), (2,3) ]
# -> [ (1,7) (9,17) ]

def merge_intervals(intervals):
    return merged
```

## Optimal Solution
```python
def merge_intervals(intervals): 
    sorted_by_lower_bound = sorted(intervals, key=lambda tup: tup[0])
    merged=[] 
    for h in sorted_by_lower_bound: 
        if not merged: 
            merged.append(h) 
        else: 
            l = merged[-1] 
            if l[1]+1 >= h[0]: 
                merged[-1] = (l[0],max(l[1],h[1])) 
            else: 
                merged.append(h) 
    return merged
```

## Alternative Solutions


## Tricks and Traps
* The sample input and output is shown in the comments, not really provided in code
* The "adjacent" part of the instructions means that `(4,7)` should be merged with `(1,3)` (since 4 is adjacent to 3), if they don't take into account the adjacency and just look at the top and bottom values, they could miss this. 

## Follow on questions
* What happens for negative numbers?
* What if no inputs are provided? `[]`
