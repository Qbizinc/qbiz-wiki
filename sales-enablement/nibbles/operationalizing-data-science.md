---
title: Operationalizing Data Science
description: 
published: true
date: 2021-08-20T22:50:31.588Z
tags: sales, sales-support, pre-sales, data-science, data-engineering
editor: markdown
dateCreated: 2021-08-19T18:27:03.419Z
---

# Operationalizing Data Science
> Work in progress
{.is-warning}


## The Data Science Assembly Line
Roadblocks to operationalizing Data Science are typically experienced by business stakeholders as lack of throughput. Data Scientists may have been hired and are doing good work, but the results are non-existent or take a very long time to materialize.

Depending on the maturity and culture of a business's organization, bottlenecks can occur at one or more of the following stages in the Data Science assembly line.

##### Jenny
- Sales process: repeat and reframe
  - "Data Science throughput is your problem"
  - "You cannot fix or improve your throughput without an assembly line"
  - assembly line is cross-functional

## 1. Enabling Data Access (EDA)

> How much time are your Data Scientists spending actually doing Data Science vs data-wrangling and cleanup?"

**Problem**: Data Scientists typically pull data from data lakes or data warehouses, both of which are ill-suited for training models.


**How Qbiz can help**: [The Feature Store](/sales-enablement/nibbles/sales-enablement/ds-feature-store)

## 2. Doing Data Science

At this stage, Data Scientists have access to the data features they need and are building and batch training models

> Are your scientists writing code with an eye toward operationalizing it? Are they spending a lot of time wrestling with the nuts and bolts of coding? Are they able to collaborate in code effectively? Do they have the right tools?

**Problem**: Data Scientists are able to build/train their models are likely working in isolation and writing code that others can't easily understand or adapt. They may be working with their tools inefficiently as well as spending too much time worrying about how to connect to data sources, how to write data out, etc.

**How Qbiz can help**: We can introduce tools and coding best practices (including reusable libraries, unit tests, and scalability tests) to streamline collaboration and make the path to production smoother


#### missing?
* what is to sell data science tooling? what's required?

## 3. Delivering models outputs to decision actors (machine actor, human actor)
> Are production systems consuming the output of your models? If so, are you able to iterate on changes to your models quickly enough to satisfy business stakeholders?

**Problem**: Data Science often faces roadblocks from IT and operations because Data Science's needs are poorly understood, systems and processes are optimized for stability, and Data Scientists lack the vocabulary to articulate their needs to IT

**How Qbiz can help**: Qbiz Data Engineers are fluent in the languages and cultures of both Data Science and IT/Operations and can therefore bridge that gap. We are able translate Data Science needs into the language, processes, and infrastructure that IT requires.




## 4. Closing the model feedback loop

> Once your models are deployed, do you have a reliable and effecient means of adjusting them?

**Problem**: Once models are deployed, it can be difficult to adjust them in production quickly enough to ensure continued performance

**How Qbiz can help**: We will work with IT stakeholders to develop safe and reliable mechanisms that enable quick iteration and performance tuning

> continuous training and deploy of models

## 5. Realtime model feedback

> Do you have tooling in place to monitor model performance in realtime or near-realtime?

**Problem**: Model performance is typically measured via batch adhoc queries and analyses. This can result in issues not being detected quickly enough to course correct in a timely way

**How Qbiz can help**: We can work with Data Science and IT to establish processes to provide continuous monitoring of model performance, appropraite alerting, and visualizations.

