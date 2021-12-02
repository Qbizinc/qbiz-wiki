---
title: Astronomer Apache Airflow Fundamentals
description: 
published: true
date: 2021-12-02T23:11:33.307Z
tags: 
editor: markdown
dateCreated: 2021-12-02T22:38:43.254Z
---

# Astronomer Apache Airflow Fundamentals
This article will help you as a study guide for the certification exam. However, I might miss some important remarks or exceptions made by the author. Feel free to contact me if something should be added

**IMPORTANT: Please finish the course before going through this guide**

## Welcome!

- Mainly setting up local environment (installing [Docker](https://docs.docker.com/desktop/mac/install/) and [Astronomer CLI](https://www.astronomer.io/docs/cloud/stable/develop/cli-quickstart))
- Note: During Astronomer CLI setup you may run into this issue detailed [here](https://forum.astronomer.io/t/astro-dev-start-error/1192)
  - Fixed using solution [here](https://forum.astronomer.io/t/buildkit-not-supported-by-daemon-error-command-docker-build-t-airflow-astro-bcb837-airflow-latest-failed-failed-to-execute-cmd-exit-status-1/857) (i.e. using `DOCKER_BUILDKIT=0 astro dev start`)
- Note: When using Docker image from course, was not able to pull up localhost in UI
  - Looks like a [known issue](https://forum.astronomer.io/t/webserver-ui-does-not-appear/1111/3); unresolved as of 11/30/21

## The Essentials

### Three Core Components of Airflow

1. Web Server: Flask server w/ interface
  - Without it, not able to monitor jobs
2. Scheduler: Heart of Airflow
  - Without it, not able to schedule/trigger any tasks
  - Can have other schedulers in case one goes down
3. Metadata Database: Stores metadata data related to airflow (i.e. users, jobs, parameters, connections, etc.)
  - Any DB that has integrations with SQLAlchemy can be used (default is SQLite)

#### Additional Components running behind the scenes
1. Executor: Defines how tasks will be executed (and on what systems) by Airflow
    - I.e. if execution done via Kubernetes, will be done using Kubernetes executor
  - By default, Airflow executes tasks sequentially (one after another)
2. Worker: Process/subprocess(es) where task is actually executed

### Most Common Architectures

1. Single Node
  - All components of Airflow (Web Server, scheduler, metastore, executer) run on same machine
    - Note: The Executor contains a separate part called a “Queue” (list of tasks to be executed in a certain order
  - Typical workflow
    - All the time
      - Web Server → Metastore (surfaces most information in UI)
    - When tasks ready to be executed
      - Scheduler → Metastore (changes task status in DB) 
      - Scheduler → Executor (created task instance object, sent to the “queue” of executor to be executed)
      - Executor → Metastore (updates status of task)
2. Multi Node
  - Components split up into different nodes
    - I.e. using Celery executor, where Scheduler/Web Server/Executor are on one node and Metastore/Queue on other
  - Note: In this set up, the “Queue” is external to the Executor
    - *In order to have this work, will need a broker system like RabbitMQ or Redis (more on this [here](https://www.educba.com/rabbitmq-vs-redis/))*
  - Worker nodes each in separate nodes as well
  - Typical workflow
    - All the time
      - Web Server → Metastore (surfaces most information in UI)
    - When tasks ready to be scheduled/executed
      - Scheduler → Metastore (changes task status in DB) 
      - Scheduler → Executor (created task instance object, sent to the “queue” of executor to be executed)
      - Executor → Queue (task info sent to queue to be picked up by worker nodes)

### Core Concepts
  - DAGs: Directed Acyclic Graphs (Data pipelines)
    - Graph with nodes/edges
    - Edges are directed, no loops
  - Operators: Tasks within DAGs
    - Object wrapper around task you want to execute
    - Three types
      1. Action Operators
         - Executes code in data pipeline (i.e. PythonOperator executes Python code)
      2. Transfer Operators
         - Transfers data from source to destination (i.e. MySQLtoPrestoOperator)
      3. Sensor Operators
         - Logic to have tasks wait until conditions met before executing (i.e. FileSensor waits until file in certain location before having other task executed)
    - When operator instantiated in a DAG, it becomes a Task
  - Task: Instance of an operator
    - When task ready to be scheduled, becomes a “Task Instance object”
  - Task Instance: Object representing specific run of a task: DAG + Task + Point in time
  - Dependencies: Edges in DAG graph
    - Specifies relationships between tasks
    - To declare dependencies between tasks
      1. (Recommended) Use “bitshift” operators (<< or >>)
         - I.e. task1 >> task2 to have task2 follow task1
      2. Use set_upstream() or set_downstream() functions
  - Workflow: All of above concepts combined (a DAG containing various operators, which themselves contain/execute various tasks, all having various dependencies, etc.)

### Task Lifecycle

In addition to Web Server, scheduler, metastore, and executer a folder with all of the DAGs (All Python files) defined is also needed. This is regularly parsed by the web server (default every 30 seconds) and scheduler (default every 5 minutes).

At the beginning of a DAG, the DAG itself as well as its individual tasks have no statuses. After parsing, if a DAG ready to be triggered the scheduler creates a DAG run object with a status of “running” and communicates the information within the DAG (overall start/execution date, task execution times, task dependencies, etc.) with the metastore as well as all necessary task information.

As soon as task *within the DAGRun object* is ready to be triggered, the scheduler creates a Task instance object with a status of “Scheduled” (the status of the task is updated in the metastore).

The scheduler sends the task instance object to the executor (or more specifically, the "queue" of the executor) with status of “Queued”  (the status of the task is updated in the metastore).

The task is now ready to be sent off to a worker node for execution with a status of “Running” (the status of the task is updated in the metastore).

Once individual tasks complete (successfully or not), the status of the task is updated in the metastore.

Once all tasks in DAG complete, the status of DAG is updated in the metastore. 

**(Update this):** Webserver updates the UI every ….?

### 