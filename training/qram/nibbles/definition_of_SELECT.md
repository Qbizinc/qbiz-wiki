---
title: Definition of SELECT
description: 
published: true
date: 2021-10-14T20:58:05.343Z
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
- Accepts one or more statements at a time. This is a `batch`. `Read`
- Executes each statement one at a time. `Eval`
- Each statement may or may not affect the internal state (stored data) in the database. In either case, the results of the statement are `Print`ed back to the user.
- `Loop`: move onto the next statement.

NB: The particulars of what happens when one statement fails (i.e., whether the batch continues or stops or something else) varies by engine, and each SQL engine has ways to specify that. This is product vendor specific.

In most SQL engines, statements are separated by semicolons. If you see a semicolon, you're seeing a statement.

Certain SQL engines don't accept batches at all. No semicolons. It's up to the user to submit statements one at a time. The user may be code in another engine, such as a Python interpreter connecting to the SQL interpreter over TCP/IP.

> @TODO: move this into sync/async article

Certain SQL engines only allow one connection at a time, such as SQLite locally. Most are multi-session: each connection is over separate TCP/IP forms a session, where statements and batches are submitted sequentially. If statements must be executed in parallel, the user must do this over a different session, which requires a different network connection. **Sessions are always sequential, one statement at a time.**

Sequential means synchronous, serial, and blocking. (Each word means a slightly different thing in CS, but here they're the same.) The session/connection is completely occupied until the batch is complete. Within a batch, each statement waits for the previous statement.

Some newer SQL engines are HTTP-over-TCP/IP. These typically take one statement at a time in the form of a single HTTP request, i.e. no sessions with batches. This varies by product vendor.

For an asynchronous example, AWS Athena has a very stateful REST-ful API where you `prepare` statements and can `store` them prior to executing. AWS Athena is completely asynchronous and non-blocking: it takes `start` and `stop` requests for statements. The UI in the AWS Console wraps sequential execution around this.

## Read-Only
`SELECT` statements are read-only commands that have to be executed whole. They're atomic; i.e. there's no way to break them into smaller pieces without changing their meaning.

```sql
WITH    product_revenue AS (
    SELECT  o.product_id
    ,       SUM(o.revenue) AS product_revenue
    FROM    order AS o
    GROUP BY    o.product_id
)
SELECT  o.product_id
,       sp.product_nm
,       o.product_revenue
FROM    o
 JOIN   (   SELECT  sp.product_id
            ,       MAX(sp.product_nm) AS product_nm
            FROM    subproduct AS sp
            GROUP BY    sp.product_id
        )   AS p
  ON    o.product_id = p.product_id
```

The `SELECT` keyword appears three times. But this is exactly **one `SELECT` statement**.

And `SELECT` statements are always read-only. `SELECT` statements are guaranteed not to change the internal state of the SQL database.

The distinction is that the `SELECT` keyword may be used in other statements, such as `INSERT` statements, which do change the internal state of the database:

```sql
INSERT INTO orders
(   product_id
,   revenue
)
SELECT  1
,       100
```

This is an `INSERT` statement. It includes the `SELECT` keyword. It is not a `SELECT` statement, so it is not guaranteed read-only. `INSERT` statements are read-write.

# SELECT Expression
`SELECT` expressions are pieces of SQL that may or may not be full statements. If they are a part of a statement, they could be taken out and executed on its own as a full `SELECT` statement:

```sql
WITH    product_revenue AS (
    SELECT  o.product_id
    ,       SUM(o.revenue) AS product_revenue
    FROM    order AS o
    GROUP BY    o.product_id
)
SELECT  o.product_id
,       sp.product_nm
,       o.product_revenue
FROM    o
 JOIN   (   SELECT  sp.product_id
            ,       MAX(sp.product_nm) AS product_nm
            FROM    subproduct AS sp
            GROUP BY    sp.product_id
        )   AS p
  ON    o.product_id = p.product_id
```

This **one `SELECT statement`** contains **three `SELECT` expressions**.

## SELECT Expression I
```sql
    SELECT  o.product_id
    ,       SUM(o.revenue) AS product_revenue
    FROM    order AS o
    GROUP BY    o.product_id
```

This is the `SELECT` expression inside the `product_revenue` CTE.

This could be executed as a `SELECT` statement on its own.

## SELECT Expression II
```sql
            SELECT  sp.product_id
            ,       MAX(sp.product_nm) AS product_nm
            FROM    subproduct AS sp
            GROUP BY    sp.product_id
```

This is the `SELECT` expression inside the `p` subquery.

This could also be executed as a `SELECT` statement on its own.

## SELECT Expression III
```sql
WITH    product_revenue AS (
    SELECT  o.product_id
    ,       SUM(o.revenue) AS product_revenue
    FROM    order AS o
    GROUP BY    o.product_id
)
SELECT  o.product_id
,       sp.product_nm
,       o.product_revenue
FROM    o
 JOIN   (   SELECT  sp.product_id
            ,       MAX(sp.product_nm) AS product_nm
            FROM    subproduct AS sp
            GROUP BY    sp.product_id
        )   AS p
  ON    o.product_id = p.product_id
```

The whole `SELECT` statement is also a `SELECT` expression.

Every `SELECT` statement is a `SELECT` expression. Not every `SELECT` expression is a `SELECT` statement.

## INSERT Statement with SELECT Expression
```sql
INSERT INTO orders
(   product_id
,   revenue
)
SELECT  1
,       100
```

This `INSERT` statement contains a `SELECT` expression:

```sql
SELECT  1
,       100
```

This `SELECT` expression could be executed on its own as a `SELECT` statement. In this case, the `SELECT` expression is included in an `INSERT` statement instead.

# SELECT Clause
`SELECT` clauses are pieces of SQL inside of `SELECT` expressions.

- Every `SELECT` expression has its own `SELECT` clause at the top-level.
- If a `SELECT` expression happens to contain other smaller `SELECT` expressions inside of it, each of those have their own top-level `SELECT` clause.
- Every `SELECT` expression has exactly one top-level `SELECT` clause.

```sql
WITH    product_revenue AS (
    SELECT  o.product_id
    ,       SUM(o.revenue) AS product_revenue
    FROM    order AS o
    GROUP BY    o.product_id
)
SELECT  o.product_id
,       sp.product_nm
,       o.product_revenue
FROM    o
 JOIN   (   SELECT  sp.product_id
            ,       MAX(sp.product_nm) AS product_nm
            FROM    subproduct AS sp
            GROUP BY    sp.product_id
        )   AS p
  ON    o.product_id = p.product_id
```

This is the `SELECT` statement, which is also a `SELECT` expression.

```sql
SELECT  o.product_id
,       sp.product_nm
,       o.product_revenue
```

This is the top-level `SELECT` clause for the `SELECT` expression.

Usually, the `SELECT` clause is everything up to the very next `FROM` clause.

- Sometimes, SQL engines let you get away with a full `SELECT` expression that doesn't have a `FROM` clause. In the `INSERT` statement example:
  ```sql
  INSERT INTO orders
  (   product_id
  ,   revenue
  )
  SELECT  1
  ,       100
  ```
  The `SELECT` expression has no `FROM` clause:
  ```sql
  SELECT  1
  ,       100
  ```
  Note that not every SQL engine supports not having a `FROM` clause in `SELECT` expressions.
- If a `SELECT` clause contains subqueries at the field/column level:
  ```sql
  SELECT  o.product_id
  ,       (SELECT SUM(i.expense) FROM inventory AS i) AS total_expense
  FROM    orders AS o
  ```
  The `FROM` clause in the `SELECT` expression of the subquery is not where the top-level `SELECT` clause ends. The full `SELECT` clause here is:
  ```sql
  SELECT  o.product_id
  ,       (SELECT SUM(i.expense) FROM inventory AS i) AS total_expense
  ```
  The `SELECT` expression:
  ```sql
  SELECT SUM(i.expense) FROM inventory as i
  ```
  has its own `SELECT` clause of:
  ```sql
  SELECT SUM(i.expense)
  ```

`SELECT` clauses may or may not be full `SELECT` expressions. If it is a `SELECT` expression, then it is executable as a `SELECT` statement, whether or not it is actually contained in a statement of a different kind, i.e. `INSERT` statement.

```sql
SELECT  1
,       100
```

This is a `SELECT` clause on its own. It happens to be a `SELECT` expression for certain SQL engines. In the SQL engines where it is a `SELECT` expression, it is always also a `SELECT` statement too.
