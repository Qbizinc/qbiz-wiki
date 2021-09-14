---
title: Astronomer DAG Authoring for Apache Airflow
description: 
published: true
date: 2021-09-14T03:52:54.126Z
tags: 
editor: markdown
dateCreated: 2021-09-13T16:48:50.416Z
---

# Astronomer DAG Authoring for Apache Airflow
This article will help you as a study guide for the certification exam. However, I might miss some important remarks or exceptions made by the author. Feel free to contact me if something should be added

## DAG Basics

### Dag Operator

The scheduler is only going to Parse a python file in the "dag" directory if the file starts with the word DAG or the word airflow, you can change this in the configuration file. To create a dag you have to use the DAG operator. 

Parameters of the DAG operator:

- Dag_id: Name of your dag (if you have a two dags with same dag_id it will not throw an error)
- Description
- start_date:  The date when your dag starts being scheduled
- schedule_interval: Defines the frequency the dag is triggered
- dagrun_timeout: The time that you dag is going to run before timingout
- tags
- catchup: Prevents backfilling your dag runs

### Dag Scheduling

Remember, the dag starts being scheduled from the start_date and will be **triggered** after every schedule_inteval. This means that if you start_date is 10:00 AM and your schedule_interval is 10 minutes, your first dag run is at 10:10 AM

Execution_date: Is the logical date and time at which the dag and its task instances run. This also acts as a unique identifier for each DAG Run.

### Cron vs Timedelta

In the schedule_interval parameter you can use Cron or a timedelta object, the difference between those is that Cron expression is stateless, whereas a timedelta is relative to the previous execution_date

### Idempotent and Deterministic
Every Dag should be Idempotent and Deteministic

- Idempotent: If you execute your dag multimple times, you should allways get the same side effect
- Deterministic: If you execute your dag with the same inputs you will get the same output

### Backfilling
You can use the CLI (with the "airflow" command) or in the UI to backfill your dags if necessary, indicating the start_date and end_date that you want to backfill. You can also clear the states of the dag runs in case you ran the dag before and you want to run the dag again

- max_active_run (DAG operator parameter): Allows you to select how many dag runs can be executed concurrently  (usefull for backfilling)

## Variables

