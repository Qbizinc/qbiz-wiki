---
title: Separate and Summarize
description: 
published: true
date: 2022-05-26T17:57:54.360Z
tags: 
editor: markdown
dateCreated: 2022-05-26T17:57:54.360Z
---

# Interview Question: Palindromes
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

def separate_and_summarize(orders):
    # your code

orders = [
    {'customer_id': 123, 'product_id': 111, 'amount': 98},
    {'customer_id': 123, 'product_id': 222, 'amount': 54},
    {'customer_id': 456, 'product_id': 111, 'amount': 76},
]

print(separate_and_summarize)

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
In this case, using the python ability to slice a string into characters with a -1 index, will reverse the string, comparing a string to it's reverse should be true for a palindrome.
```python
def is_palindrome(mystring):
    return mystring == mystring[::-1]

mylist = ["airplane", "kayak", "racecar", "anna"]
for word in mylist:
    print(f'{word} is a palindrome? {is_palindrome(word)}')
```

## Alternative Solutions
Alternative approaches:
* Looping over the characters and comparing the start to the end 
* Reversing the string using something other than [::-1] 

## Tricks and Traps
* The sample data provided is a list, but the method seems to take a string
  * Python does not have strong typing by default so if they just pass mylist into is_palindrome they may not get what they expected
* There is no call provided to is_palindrome, so someone may write code that seems to do nothing if they don't print the results

## Follow on questions
* Add case sensitivity to the inputs i.e. `"Anna"` instead of `"anna"`
* Add punctuation to the inputs i.e. `"A man, A plan, A canal, Panama!"`

