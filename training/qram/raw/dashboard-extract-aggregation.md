---
title: Don't Aggregate Your Dashboard Extract
description: 
published: true
date: 2021-07-30T01:09:41.376Z
tags: aggregation, business-intelligence
editor: markdown
dateCreated: 2021-07-30T00:27:32.648Z
---

When working with SQL we often like to perform many calculations and aggregations to help us draw some snap conclusions. We might be tempted to include those same calculations in a dashboard, but that would actually cause many logical issues. **Often it is better to only include the lowest level of your data that is necessary in an extract**. In a simple example below, we have some payment information with an extra dimension that we want to group by in our visualization.

![dashboard_aggregation_table1.png](/dashboard_aggregation_table1.png)

Perhaps we've been asked to keep track of the mean amount paid per user by country, so you write in SQL:
```sql
SELECT country
     , MEAN(amount_paid) AS mean_amount_paid
FROM table1
GROUP BY 1
```

And you get something like:

![dashboard_aggregation_results1.png](/dashboard_aggregation_results1.png)

Knowing that your dashboard users will also want to see the mean amount paid by all users, regardless of country, you add a filter option for ALL countries and miss a crucial step: your visualization client doesn't know that it's working with a column that has already been aggregated. You cannot take the mean of two means. **You must make sure that your data extract is both commutative and associative.**

In the above example it would have been better to provide the results of the **SUM** and **COUNT** functions to then feed into the visualization client to calculate the mean. This would provide us with the flexibility of calculating the mean at the level we want, and prevent our extract from becoming unmanageably large due to feeding in the raw tables.