---
title: Vordo
description: 
published: true
date: 2022-02-15T20:49:28.085Z
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
SELECT TBD
-- 3. names of salespeople with 2 or more orders.
```
```sql
-- 4.  dates of the oldest and most recent order made to Beebee

-- 5a. names of salespeople that have an order with Vordo
-- 5b. names of all salespeople that do not have any order with Vordo.
```
## Alternative Solutions

## Tricks and Traps
* None of the tables have primary keys, despite a field being named id
* There are two salespeople with the same id
* Cailin has no orders
* The call was coming from in the house
* The key fields don't line up between the tables (i.e. `salespeople_id = id` or `order_id=id`

## Follow on questions

