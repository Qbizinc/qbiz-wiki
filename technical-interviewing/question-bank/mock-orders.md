---
title: Mock Order Data
description: 
published: true
date: 2022-02-28T18:22:45.964Z
tags: interview_question
editor: markdown
dateCreated: 2022-02-25T16:10:57.948Z
---

# Interview Questions: Mock Orders
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

##### 1. Show the names of all salespeople who sold to customer 'Mante-Jenkins'
###### Solution
```sql
SELECT DISTINCT s.name
FROM SALESPEOPLE s 
JOIN ORDERS o on s.id = o.rep
JOIN CUSTOMERS c on o.customer = c.cust_id
WHERE c.name = 'Mante-Jenkins'
```
###### Expected
||      name      ||
|-----------------|
| Cheri Odom |
| Cyrillus Cokly |
| Erroll Rymmer |
| Eyde Thurlow |
| Wynny McCullogh |

-----
##### 2. Show the names of all salespeople who did not have an order with customer 'Mante-Jenkins'
###### Solutions
Inner Query Alternative
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
CTE Alternative
```sql
WITH mj_sales as (
    SELECT DISTINCT s2.name
    FROM SALESPEOPLE s2 
    JOIN ORDERS o on s2.id = o.rep
    JOIN CUSTOMERS c on o.customer = c.cust_id
    WHERE c.name = 'Mante-Jenkins'
)
SELECT DISTINCT s.name 
FROM SALESPEOPLE s 
WHERE s.name not in (SELECT name from mj_sales)
```
Join CTE Alternative
```sql
WITH mj_sales as (
    SELECT DISTINCT s2.name
    FROM SALESPEOPLE s2 
    JOIN ORDERS o on s2.id = o.rep
    JOIN CUSTOMERS c on o.customer = c.cust_id
    WHERE c.name = 'Mante-Jenkins'
)
SELECT DISTINCT s.name 
FROM SALESPEOPLE s
LEFT JOIN mj_sales on s.name = mj_sales.name
WHERE mj_sales.name IS NULL;
```

###### Expected

||     name      ||
|---------------|
| Mark Saderson |

----------

##### 4. Who had the most sales for product 'Aonyx cinerea'
###### Potential Clarifications
There is some intentional vagueness in the wording of the question, if the candidates
* What does "most sales mean"?  How should it be measured?
  * Let's just count
* Should the count be of orders or order lines?
  * Either is fine, in this case the answer will be the same
  * This does lead to a potential follow on question which can speak to data quality and business process 
    * Are there any reps who had multiple lines for 'Aonyx cinerea' within the same order?  Why do we think that is?

###### Solution
Limit 1 
```sql
SELECT s.name, count(ol.id) as num_order_lines, count(distinct o.id) as num_orders 
FROM SALESPEOPLE s 
JOIN ORDERS o on s.id = o.rep
JOIN ORDER_LINES ol on o.id = ol.order_id
JOIN PRODUCTS p on ol.product = p.id
WHERE p.name = 'Aonyx cinerea'
GROUP BY s.name
ORDER BY num_order_lines desc 
LIMIT 1
```

RANK Alternative
```sql
SELECT * FROM ANSWERS_TO_BE_WRITTEN
```

HAVING Alternative
```sql
SELECT * FROM ANSWERS_TO_BE_WRITTEN
```

###### Expected

| name | num_order_lines | num_orders |
|------|---------------|------------|
| Wynny McCullogh | 8 | 6 |

----------

##### 5. Show order totals by customer


##### 6. Show order totals by salesperson


##### 7. Show order totals by product


##### 8. What order had the highest profit margin?  Who made that sale? Who was it to?

```sql
```

## Tricks and Traps
* 


## Follow on questions

