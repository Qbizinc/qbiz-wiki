---
title: Auto DAG Maker for Airflow
description: 
published: true
date: 2023-03-01T18:52:52.722Z
tags: bench, opensource tools airflow dag, opensource, tools, airflow, dags
editor: markdown
dateCreated: 2023-02-28T17:46:13.080Z
---

# Description
DAGs with dependenices from YAML/JSON/etc to facilitate version-controlled data pipeline creation by analysts/non-DEs.

# Meta

**Advocates**: Jay, Gustavo
**Internal value**: High
**External value**: High
**LOE**: 4 - Multiple people and/or weeks

# Comments

- We could also target dbt, which can be intimidating to non-DEs
- We need to be careful about scope -- we don't want to create an entire software package.
- Additionally, there are others playing in this space -- we don't want to duplicate efforts

- We potentially create an auto dag maker for both dbt and airflow, that uses RETOOL as a front-end? Maybe start w/ one and then roll out the other one?

### Manual inputs
- User pastes SQL in a nice GUI
- User picks betweens truncate load / incremental
- User clicks on BUILD button

### TOOL
- Automatically detects upstream dependencies and builds sensors. Flags unavailable sensors by pinging all code contributors of that DAG
- Automatically builds post operators so other sensors can discover
- Automatically jinjafies database.schemas and ds 
- Builds DAG / dbt model with DQ + testing harnesses in place

