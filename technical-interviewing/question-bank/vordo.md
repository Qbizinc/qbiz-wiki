---
title: Vordo
description: 
published: true
date: 2022-02-22T18:12:28.488Z
tags: interview_question
editor: markdown
dateCreated: 2022-02-15T20:49:28.085Z
---

# Interview Question: Vordo
> **Language: SQL**
{.is-warning}

> **Difficulty: Low**
{.is-success}

## Question Text
This series of questions builds up a simple SQL dataset and asks
the candidate to perform queries that increase in complexity.

### Questions
1. Simple Where clause filter
1. Simple Join between two tables
1. Having clause or CTE to get the sum
1. Two aggregates in the same query
5. Two part question:
5a. Three way join
5b. Related to 5a, but needs to be the opposite set


```sql
DROP TABLE IF EXISTS salespeople;
CREATE TABLE salespeople (
  id INT,
  name VARCHAR(100),
  age INT,
  salary INT
);
INSERT INTO salespeople VALUES
  (1, 'John',   61, 120000),
  (2, 'Ava',    34, 135000),
  (5, 'Cailin', 28,  70000),
  (7, 'Mike',   41,  52000),
  (8, 'Ian',    57,  80000),
  (1, 'Joe',    38,  38000)
;

-- explain verbally what the above does


-- Given the table above, type in a query that prints :
-- 1.  names and ages of salespeople with a salary of 80000 or greater.


DROP TABLE IF EXISTS orders;
CREATE TABLE orders (
  id INT,
  order_date DATE,
  cust_id INT,
  salespeople_id INT,
  amount INT
);
INSERT INTO orders VALUES
  (10, '2011-08-02', 4, 2,  540),
  (20, '2012-01-30', 4, 8, 1800),
  (30, '2009-07-14', 9, 1,  460),
  (40, '2010-02-02', 7, 2, 2400),
  (50, '2011-07-23', 6, 7,  600),
  (60, '2012-02-28', 6, 7,  720),
  (70, '2011-05-06', 6, 7,  150)
;

-- 2. names of each salespeople, their number of orders, and total amount
-- 3. names of salespeople with 2 or more orders.


DROP TABLE IF EXISTS customers;
CREATE TABLE customers (
  id INT,
  name VARCHAR(100),
  city VARCHAR(100),
  industry_type VARCHAR(100)
);
INSERT INTO customers VALUES
  (4, 'Vordo',  'Maryville', 'J'),
  (6, 'Beebee', 'Appletown', 'J'),
  (7, 'Sono',   'Oaktown',   'B'),
  (9, 'Frog',   'Lillypad',  'B')
;

-- 4.  dates of the oldest and most recent order made to Beebee

-- 5a. names of salespeople that have an order with Vordo
-- 5b. names of all salespeople that do not have any order with Vordo.
```

## Optimal Solution

```sql
-- 1.  names and ages of salespeople with a salary of 80000 or greater.
SELECT s.name, s.age 
FROM salespeople s
WHERE s.salary >= 80000;
```
```sql
-- 2. names of each salespeople, their number of orders, and total amount
SELECT s.name, count(o.id) as num_orders, sum(o.amount) as total_amount
FROM salespeople s
LEFT JOIN orders o on o.salespeople_id = s.id
GROUP BY s.name;
-- 3. names of salespeople with 2 or more orders.
SELECT s.name
FROM salespeople s
LEFT JOIN orders o on o.salespeople_id = s.id
GROUP BY s.name
HAVING count(o.id) >= 2
;

```
```sql
-- 4.  dates of the oldest and most recent order made to Beebee
SELECT min(o.order_date) as oldest_order, max(o.order_date) as most_recent_order
FROM orders o
JOIN customers c on o.cust_id = c.id
WHERE c.name = 'Beebee';

-- 5a. names of salespeople that have an order with Vordo
SELECT DISTINCT s.name
FROM orders o 
JOIN salespeople s on o.salespeople_id = s.id
JOIN customers c on o.cust_id = c.id
WHERE c.name = 'Vordo';
-- 5b. names of all salespeople that do not have any order with Vordo.
SELECT s2.name
FROM salespeople s2
WHERE s2.name not in (
    SELECT DISTINCT s.name
    FROM orders o 
    JOIN salespeople s on o.salespeople_id = s.id
    JOIN customers c on o.cust_id = c.id
    WHERE c.name = 'Vordo'
)
```
## Alternative Solutions

## Tricks and Traps
* None of the tables have primary keys, despite a field being named id
* The call was coming from in the house
* The key fields don't line up between the tables (i.e. `salespeople_id = id` or `order_id=id`
* There are "primary key" conflicts in the salespeople table (ids are not primary keys)
* Cailin has no orders, so question 2 needs a left join
* Common mistake on 5b is to show all orders for customers that are not Vordo


## Follow on questions

