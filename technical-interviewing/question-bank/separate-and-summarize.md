---
title: Separate and Summarize
description: 
published: true
date: 2022-10-18T20:09:50.582Z
tags: 
editor: markdown
dateCreated: 2022-05-26T17:57:54.360Z
---

# Interview Question: Separate and Summarize
> **Language: Python3**
{.is-info}

> **Difficulty: Low**
{.is-success}

## Question Text

```python
# Write a function that accepts a list of orders. Each order is a dictionary with the keys customer_id,
# product_id, and amount. Return a dictionary, where each key is an unique customer_id.
# The value for each customer_id key must be a dictionary with the keys amount and orders.
# amount is the total amount for all of the orders belonging to that customer_id. orders is a list
# containing all of the original orders that belong to that customer_id.

# For example:

import pprint

def separate_and_summarize(orders):
    # your code

orders = [
    {'customer_id': 123, 'product_id': 111, 'amount': 98},
    {'customer_id': 123, 'product_id': 222, 'amount': 54},
    {'customer_id': 456, 'product_id': 111, 'amount': 76},
]

output = separate_and_summarize(orders)
pprint.PrettyPrinter(indent=4).pprint(output)

# Expected Output:
# {
#     123: {
#         'amount': 152,
#         'orders': [
#             {'customer_id': 123, 'product_id': 111, 'amount': 98},
#             {'customer_id': 123, 'product_id': 222, 'amount': 54},
#         },
#     },
#     456: {
#         'amount': 76,
#         'orders': [
#             {'customer_id': 456, 'product_id': 111, 'amount': 76},
#         },
#     },
# }
```

## Optimal Solution

### Imperative Programming Approach
```python
def separate_and_summarize(orders):
    output = {}
    for order in orders:
        customer_id = order['customer_id']
        if customer_id in output:
            output[customer_id]['amount'] += order['amount']
            output[customer_id]['orders'].append(order)
        else:
            output[customer_id] = {
                'amount': order['amount'],
                'orders': [
                    order,
                ],
            }
    return output
```

### Functional Programming Approach

This approach shows someone thinks functionally, i.e. most likely has a background in a FP like Scala.
They would likely also write more efficient PySpark code.

```python
import itertools
import operator

def separate_and_summarize(orders):
    orders_by_customer_id_tuple_iter = itertools.groupby(orders, operator.itemgetter('customer_id'))
    def summarize(orders):
        orders = list(orders)
        amount = sum(map(orders), operator.itemgetter('amount'))
        summary = {'amount': amount, 'orders': orders}
        return summary
    summaries_by_customer_id_iter = map(orders_by_customer_id_tuple_iter, summarize)
    summaries_by_customer_id = dict(summaries_by_customer_id_iter)
```

## Alternative Solutions

?

## Tricks and Traps
* In the imperative approach, the initial dict for each customer_id needs to have a list(order) as its starting 'orders' list.
  Otherwise, the .append(order) call will not work. (This comes up a lot in the HackerRank for good Python candidates.)

## Follow on questions

Not recommended.
