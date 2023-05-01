---
title: Architecture Solution Assessment Guide
description: Guide for assessing and evaluating client infrastructure
published: true
date: 2023-05-01T21:27:55.901Z
tags: 
editor: markdown
dateCreated: 2023-04-28T23:02:08.075Z
---

# Architecture Solution Assessment Guide

This guide provides a step by step process for assessing client infrastructure and needs to capture all the requirements and constraints of a new cloud based software architecture.

### Current state

- What is the current state of the client architecture? 
- Where are the pain points, bottlenecks, blind spots, etc.? 
- Does the architecture have proper security measures in place? 
- Are any SLOs/SLAs defined, and is this architecture meeting them? 
- What is the level of staffing support for this architecture if any? 
- Include in here any analytics/ML/etc. use cases that are NOT being achieved by this architecture that the client would like.

### Customer needs

- What are the needs of the end users of this architecture? 
- Are the end users paying customers or internal employees? 
- What are the SLOs/SLAs for this architecture if any? 
- Do they require sub second latency, or is some latency acceptable? 
- Are there strict geographical requirements for where data is stored? 

### Desired future state

- What is the desired state of the client architecture? 
- How does this new architecture address the pain points, bottlenecks, blind spots, etc. mentioned above? 
- Will there be a team managing this infrastructure or not (i.e. fully managed vs serverless vs codeless offerings)? 
- Include here any analytics/ML/etc. use cases that this architecture achieves that was not possible before with the old architecture.

#### Constraints

- What are the constraints to being able to successfully spin up the new architecture? 
- Is there a specific budget and/or timeframe that needs to be adhered to? 
- Are there complicated legacy systems that need to be migrated to the cloud/connected to the cloud? 
- Is the team managing this infrastructure properly staffed and/or trained to maintain this architecture?

#### Data types and formats

- What are the various data types and formats being collected? HOW is the data being copllected (i.e. batch or streaming)? 
- Does this data need to be transformed into a standardized format? Is there any opportunity for data type optimization (i.e. turning a general char field into a char(n) field) or table optimization (i.e. turn multiple descriptor fields into one ARRAY field)? 
- Make sure to differentiate between raw and processed data, any specialized requirements for formatting/types, etc.

#### Data sources and sinks

- What are the various data sources and sinks? 
- Is data distributed across silos or all in one place? 
- What is the access/security model of each data source, and does this model need to be updated with the new architecture? 
- Do any of the sources and sinks need to be modified for the new architecture?

#### Data pipelines

- What are the various data pipelines in the current architecture? 
- Are they written in an efficient way, or is everything manual and disorganized? 
- Are they orchestrated in a reliable way, or is everything manual and human driven? 
- Are the pipelines written in Python or another language? 
- How much work would it require to rewrite the pipelines in a format that could be automated/orchestrated (i.e. executed in Airflow)? 
- Are there any unit tests for data validation (i.e. data type, NULLs, etc.)?

#### Networking and security

- Are there strict security requirements for this architecture? 
- Is public cloud okay for this architecture, or is the private cloud (i.e. AWS VPC) needed? 
- Are there any opportunities to improve security through new architecture (i.e. implement access permissions model, deprecating redundant architecture, etc.)?
