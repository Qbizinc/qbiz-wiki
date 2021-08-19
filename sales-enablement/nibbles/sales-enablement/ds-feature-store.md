---
title: The Data Science Feature Store
description: 
published: true
date: 2021-08-19T19:10:56.294Z
tags: sales-support, pre-sales, data-science-enablement
editor: markdown
dateCreated: 2021-08-19T18:32:29.337Z
---

See [Operationalizing Data Science](/sales-enablement/nibbles/operationalizing-data-science)

# What
A feature store is a curated collection of data intended for model training. Each level of event granularity may imply a unique data model, with its own set of dimensional data

# Why
* Data Warehouses are intended for reporting, not model training. The data may be too aggregated, dimensionalized, and at an inappropriate/rigid event granularity
* Data Lakes are unorganized, catch-all data stores. 80% may be garbage. Data Scientists need to make sense of the mess, clean the data, and extract the appropriate features. Event granularity may be haphazard. Dimensional data may not exist

--- 
> What is a feature store? "A discipline where you name a granularity of action and what falls out of that is a set of analysis that leads to an ETL design that leads to an ETL implementation that leads to a data set at the appropriate level of granularity and the appropriate features that are independent variables and associated dependent variables so you can do training"
