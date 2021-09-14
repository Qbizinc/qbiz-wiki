---
title: Astronomer DAG Authoring for Apache Airflow
description: 
published: true
date: 2021-09-14T06:28:21.257Z
tags: 
editor: markdown
dateCreated: 2021-09-13T16:48:50.416Z
---

# Astronomer DAG Authoring for Apache Airflow
This article will help you as a study guide for the certification exam. However, I might miss some important remarks or exceptions made by the author. Feel free to contact me if something should be added

**IMPORTANT: Please finish the course before going through this guide**

## DAG Basics

### Dag Operator

The scheduler is only going to Parse a python file in the dag directory if the file starts with the word DAG or the word airflow, you can change this in the configuration file. To create a dag you have to use the DAG operator. 

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

- Idempotent: If you execute your dag multimple times, you should allways get the same side effects
- Deterministic: If you execute your dag with the same inputs you will get the same output

### Backfilling
You can use the CLI (with the "airflow" command) or the UI to backfill your dags if necessary, indicating the start_date and end_date that you want to backfill. You can also clear the states of the dag runs in case you ran the dag before

- max_active_run (DAG operator parameter): Allows you to select how many dag runs can be executed concurrently  (usefull for backfilling)

## Variables
Variables are a key-value object that is stored in the metadeta database in airflow. Dags can access this variables using the function Variable.get(*key_of_variable*). Variables are usefull when different dags interact with the same API endipoint or the same entry bucket. 

To create a variable you can use the UI, the REST API or the CLI. You can also hide variables from users using the "_secret" suffix

### Properly Fetch your Variables
Do not fecth variables outside of tasks as each time you dag is parsed, you will create a connection in the metedata database, even if you dag is not running yet. 

If you need to retrieve different variables for the same dags; to avoid connecting multiple times to the metadata database, you can create a variable with a json value. To fetch this type of variables you can use two methods:

- Variable.get(key_of_variable, deserialize_json=True)
- {{var.json.key_of_variable.key_of_json}} (Jinja Template)

### Environment Variables
If you need to completely hide a variable from your users, you can fetch an Envorimental Variable using the same methods as before. This also removes the need of connecting to the metadata database

[More:] https://academy.astronomer.io/astronomer-certification-apache-airflow-dag-authoring-preparation/891132

## XCOMS and TaskFlow API


### Templating Jinja Engine

Templating is a powerful concept in Airflow to pass dynamic information into task instances at runtime. For example, say you want to print the day every time you run a task:

BashOperator(
    task_id="print_day",
    bash_command="echo Today is {{ ds }}",
)

In this example, the value in the double curly braces {{ }} is our templated code to be evaluated at runtime

By default, the argument parameters of the PostgresOperator is not templated. But remember that there is a way to change this in your dag (Check Video)

### XCOMs

XCom (short for cross-communication) is a native feature within Airflow. XComs allow tasks to exchange task metadata or small amounts of data. They are defined by a key, value, and timestamp.

XComs can be "pushed", meaning sent by a task, or "pulled", meaning received by a task. When an XCom is pushed, it is stored in Airflow's metadata database and made available to all other tasks. Any time a task returns a value (e.g. if your Python callable for your PythonOperator has a return), that value will automatically be pushed to XCom. Tasks can also be configured to push XComs by calling the xcom_push() method. Similarly, xcom_pull() can be used in a task to receive an XCom.

XCom cannot be used for passing large data sets between tasks. The limit for the size of the XCom is determined by which metadata database you are using:

Postgres: 1 Gb
SQLite: 2 Gb
MySQL: 64 Kb

### TaskFlow API

The TaskFlow API is a functional API that allows you to explicitly declare message passing (Xcom's between tasks) while implicitly declaring task dependencies

TaskFlow API Functionality Includes:

- XCom Args, which allow task dependencies to be abstracted and inferred as a result of the Python function invocation
- A task decorator that automatically creates PythonOperator tasks from Python functions and handles variable passing

### Decorators
- Task decorator (@task): allows users to convert any Python function into a task instance using PythonOperator
- DAG decorator (@dag): allows users to instantiate the DAG without using a context manager
- TaskGroup decorator (@task_group)

Remember how to create dependencies in "Taskflow API tasks": store_data_task(process_data(extract_data()))

## Grouping Tasks

### SubDags (Hard way and not usefull)

Just remember that you can group tasks and create dependencies between two different dags with this method

### TaskGroups

You can instantiate a Task Group using a with statement and the TaskGroup function (providing allways a group_id). Inside the TaskGroup, you can define the tasks and their respective dependencies.

To simplify your code, you can allways put your grouped task in another folder inside the dag directory. You can also create nested TaskGroups (Groups inside another Group)

## Advanced Concepts

## DAG Dependencies

### ExternalTaskSensor

A sensor that waits for an execution of a task in a different dag (external dag)

### TriggerDagRunOperator

Helpful if you want to trigger DAG A from DAG B.
