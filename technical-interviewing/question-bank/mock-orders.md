---
title: Mock Order Data
description: 
published: true
date: 2022-02-28T17:08:04.300Z
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

### Question/Solution/Expected

##### Show the names of all salespeople who sold to customer 'Mante-Jenkins'

```sql
SELECT DISTINCT s.name
FROM SALESPEOPLE s 
JOIN ORDERS o on s.id = o.rep
JOIN CUSTOMERS c on o.customer = c.cust_id
WHERE c.name = 'Mante-Jenkins'
```

```
      name       
-----------------
 Cheri Odom
 Cyrillus Cokly
 Erroll Rymmer
 Eyde Thurlow
 Wynny McCullogh
```

##### Show the names of all salespeople who did not have an order with customer 'Mante-Jenkins'
```sql
SELECT DISTINCT s.name 
FROM SALESPEOPLE s 
WHERE s.name not in (
    SELECT DISTINCT s2.name
    FROM SALESPEOPLE s2 
    JOIN ORDERS o on s2.id = o.rep
    JOIN CUSTOMERS c on o.customer = c.cust_id
    WHERE c.name = 'Mante-Jenkins'
)
```

```
     name      
---------------
 Mark Saderson
```


-- 4. Who had the most sales for product 'Aonyx cinerea'


-- 5. Show order totals by customer


-- 6. Show order totals by salesperson


-- 7. Show order totals by product


-- 8. What order had the highest profit margin?  Who made that sale? Who was it to?

```sql
```

## Tricks and Traps
* 


## Follow on questions

