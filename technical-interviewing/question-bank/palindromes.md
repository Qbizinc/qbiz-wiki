---
title: Palindromes
description: 
published: true
date: 2022-02-15T20:03:00.285Z
tags: interview_question
editor: markdown
dateCreated: 2022-02-15T19:46:32.571Z
---

# Interview Question: Palindromes
> **Language: Python3**
{.is-info}

> **Difficulty: Low**
{.is-success}

## Question Text

```python
# A palindrome is a sequence of characters which reads the same 
# backward as forward, such as madam or racecar.
#
# Write a simple python function that tests if a string is a palindrome. 

def is_palindrome(mystring):
    # your code here


mylist = ["airplane", "kayak", "racecar", "anna"]
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

