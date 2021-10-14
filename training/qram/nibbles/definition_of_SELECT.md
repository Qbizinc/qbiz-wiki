---
title: Definition of SELECT
description: 
published: true
date: 2021-10-14T17:54:09.154Z
tags: 
editor: markdown
dateCreated: 2021-10-14T17:54:09.154Z
---

# Summary

`SELECT` is a keyword in Structured Query Language (SQL).

As a keyword, it is used in three contexts:
- `SELECT` statement
- `SELECT` expression
- `SELECT` clause

To clear confusion, let's define each.

# SELECT Statement
A `statement` is a singular command that is executed by the SQL interpreter.

SQL is always implemented in a `Read-Eval-Print-Loop` (`REPL`), so every SQL engine is an interpreter.

This means that the SQL engine:
- Accepts one or more statements at a time. This is a `batch`.
- Executes each statement one at a time.
- Each statement may or may not affect the internal state (stored data) in the database. In either case, the results of the statement are `Print`ed back to the user.
- `Loop`: move onto the next statement.

NB: The particulars of what happens when one statement fails (i.e., whether the batch continues or stops or something else) varies by engine, and each SQL engine has ways to specify that.

In most SQL engines, statements are separated by semicolons. If you see a semicolon, you're seeing a statement.

Certain SQL engines don't accept batches at all. No semicolons. It's up to the user to submit statements one at a time. The user may be code in another engine, such as a Python interpreter over TCP/IP to the SQL interpreter.

## Read-Only
`SELECT` statements are read-only commands that have to be executed whole. They're atomic; i.e. there's no way to break them into smaller pieces without changing their meaning.

```sql
WITH    product_revenue AS (
    SELECT  o.product_id
    ,       SUM(o.revenue) AS product_revenue
    FROM    order AS o
    GROUP BY    o.product_id
)
SELECT  
FROM    o
 JOIN   (   SELECT  sp.product_id
            ,       MAX(sp.product_nm) AS product_nm
            FROM    subproduct AS sp
            GROUP BY    sp.product_id
        )   AS p
  ON    o.product_id = p.product_id
```