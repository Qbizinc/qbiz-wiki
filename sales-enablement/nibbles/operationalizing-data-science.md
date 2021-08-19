---
title: Operationalizing Data Science
description: 
published: true
date: 2021-08-19T18:27:03.419Z
tags: sales, sales-support, pre-sales, data-science, data-engineering
editor: markdown
dateCreated: 2021-08-19T18:27:03.419Z
---

# Operationalizion Stages
## 1. Enabling Data Access (EDA)


**Problem**: Data Scientists typically pull data from data lakes or data warehouses, both of which are ill-suited for training models.
> How much time are your Data Scientists spending actually doing Data Science vs data-wrangling and cleanup?"

**How Qbiz can help**: The Feature Store

* Data Warehouses are intended for reporting, not model training. The data may be too aggregated, dimensionalized, and at an inappropriate/rigid event granularity
* Data Lakes are unorganized, catch-all data stores. 80% may be garbage. Data Scientists need to make sense of the mess, clean the data, and extract the appropriate features. Event granularity may be haphazard. Dimensional data may not exist
* A feature store is a curated collection of data intended for model training. Each level of event granularity may imply a unique data model, with its own set of dimensional data

> What is a feature store? "A discipline where you name a granularity of action and what falls out of that is a set of analysis that leads to an ETL design that leads to an ETL implementation that leads to a data set at the appropriate level of granularity and the appropriate features that are independent variables and associated dependent variables so you can do training"
{.is-info}


## 2. Doing Data Science

* At this stage, Data Scientists have access the data features they need and are running batch
