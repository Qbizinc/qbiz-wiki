---
title: Mock Order Data
description: 
published: true
date: 2022-03-03T22:58:23.138Z
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
|      name      |
| :------------: |
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

|     name      |
| :-----------: |
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
WITH ranked_orders as (
     SELECT s.name, 
            count(ol.id) as num_order_lines, 
            count(distinct o.id) as num_orders,
            rank() over (order by count(ol.id) desc) as order_rank
       FROM SALESPEOPLE s 
       JOIN ORDERS o on s.id = o.rep
       JOIN ORDER_LINES ol on o.id = ol.order_id
       JOIN PRODUCTS p on ol.product = p.id
      WHERE p.name = 'Aonyx cinerea'
   GROUP BY s.name
)
SELECT name, num_order_lines, num_orders
FROM ranked_orders
WHERE order_rank = 1
```

###### Expected

| name | num_order_lines | num_orders |
| :----: | -------------: | ----------: |
| Wynny McCullogh | 8 | 6 |

----------

##### 5. Show order totals by customer
```sql
SELECT c.name as cust_name,
       sum(o.total) as total_order_amount
FROM   ORDERS o
JOIN   CUSTOMERS c on o.customer = c.cust_id
GROUP BY c.name
```

##### 6. Show order totals by salesperson
```sql
SELECT s.name as rep_name,
       sum(o.total) as total_order_amount
FROM   ORDERS o
JOIN   SALESPEOPLE s on o.rep = s.id
GROUP BY s.name
```


##### 7. Show order totals by product
```sql
SELECT p.name as product_name,
       sum(o.total) as total_order_amount
FROM   ORDERS o
JOIN   ORDER_LINES ol on ol.order_id = o.id
JOIN   PRODUCT p on ol.product = p.id
GROUP BY p.name
```


##### 8. What order had the highest profit margin?  Who made that sale? Who was it to?
This one is more complex than it seems on the surface because the order lines contain the list price and the cost, but not the amount it contributed to the order.

```sql
with total_order_cost as (
    SELECT o.id,
           o.total,
           o.rep,
           o.customer,
           sum(ol.cost) as total_order_line_cost,
           count(ol.id) as num_lines
    FROM   ORDERS o
    JOIN   ORDER_LINES ol on ol.order_id = o.id
  GROUP BY o.id, o.total, o.rep, o.customer 
)
select toc.id, 
       toc.total as order_total, 
       toc.total_order_line_cost, 
       toc.total - toc.total_order_line_cost as net_profit, 
       (((toc.total - toc.total_order_line_cost)/total_order_line_cost)*100)::int as total_order_profit_percent,

       s.name as salesperson_name,
       c.name as customer_name
FROM total_order_cost toc
JOIN SALESPEOPLE s on toc.rep = s.id
JOIN CUSTOMERS c on toc.customer = c.cust_id
order by net_profit desc 
limit 1
```

| id | total  | total_order_line_cost | net_profit | total_order_profit_percent | salesperson_name | customer_name |
---: | ------: | -----------------: | -----------: | -----: | :----------------| :---------------
 77 | 480.58 |                 36.45 |     444.13 | 1218 | Cyrillus Cokly   | Klocko-Osinski

## Tricks and Traps
* 


## Follow on questions

