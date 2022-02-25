---
title: Mock Order Data
description: 
published: true
date: 2022-02-25T16:10:57.948Z
tags: interview_question
editor: markdown
dateCreated: 2022-02-25T16:10:57.948Z
---

# Interview Question: Vordo
> **Language: SQL**
{.is-warning}

> **Difficulty: Medium**
{.is-warning}

## Question Text
There are two "Questions" in CodingView for this
1. Mock Order Data For Multiple Questions (PostgreSQL)
1. Multiple Question about Mock Order Data (PostgreSQL)

The first "question" has create table statements and about 400 inserts, this sets up the schema and data for the other "question".  This data set is meant for exploratory query purposes and the questions are focused that way - describing business problems that the fictional company might ask to have solved.  With this data set, the questions can range from the simple (i.e. "How may orders did {Cust X} place?") to the complex (i.e. "Which order had the highest profit margin?")

The following section lists some sample questions along with the expected SQL to answer those questions.

### Question

```sql
```

### Solution

```sql
```

```sql
```
```sql
```

## Tricks and Traps
* None of the tables have primary keys, despite a field being named id
* The key fields don't line up between the tables (i.e. `salespeople_id = id` or `order_id=id`
* There are "primary key" conflicts in the salespeople table (ids are not primary keys)
* Cailin has no orders, so question 2 needs a left join
* Common mistake on 5b is to show all orders for customers that are not Vordo


## Follow on questions

