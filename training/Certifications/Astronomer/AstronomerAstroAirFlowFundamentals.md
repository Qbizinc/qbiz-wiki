---
title: Astronomer Apache Airflow Fundamentals
description: 
published: true
date: 2022-10-07T21:34:17.990Z
tags: 
editor: markdown
dateCreated: 2021-12-02T22:38:43.254Z
---

# Astronomer Apache Airflow Fundamentals
This article will help you as a study guide for the certification exam. However, I might miss some important remarks or exceptions made by the author. Feel free to contact me if something should be added

**IMPORTANT: Please finish the course before going through this guide**

## Registering for the Course: 
[Prep course + Exam](https://academy.astronomer.io/plan/astronomer-certification-for-apache-airflow-fundamentals-exam). Promo code `cert-free-airflow-fundamentals` will grant free access to the course and exam. 

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

### Installing Apache Airflow
Included instructions on manual local installation of Airflow. Installing Airflow on Docker is easiest, but need to be able to do it manually as well. I unfortunately was not able to get it to work myself :(

### Extras and Providers
These both extend the “core functionalities” of Airflow.

Extras allow you to install set of dependencies needs for a full feature. For example, install Kubernetes extras to be able to launch Kubernetes.

Providers are a lightweight version of Extras in the the use case of only needing some operators/hooks/some simple functionality (i.e. Postgres). Providers can be updated independently of Airflow.

### Upgrading Apache Airflow (steps)
1. Make a backup of your metadata DB
2. Make sure there are no deprecated features in the DAG code
3. Pause all DAGs, make sure none are running
4. Upgrade airflow
   - I.e. pip install "apache-airflow[any_extra]==2.0.1" --constraint constraint-file
5. Upgrade the DB
   - Airflow db upgrade
6. Restart the scheduler/webserver and worker(s)

## Interacting with Apache Airflow

### The 3 Ways
1. User Interface (UI): Most common/used way of interacting with Airflow
   - Check job logs, history
2. Command Line Interface (CLI)
   - Test tasks
   - Upgrade airflow
   - Initialize Airflow
   - Decent backup if UI is down
3. REST API: Leverage additional functionality on top of airflow
  - I.e. If you have front end, and customer clicks on a button then trigger a DAG

Between this section and the next are useful videos showing various aspects of the Airflow UI (DAGs view, Tree view, etc.). Give them a thorough watch if you've never used Airflow.

### Astronomer CLI Commands to Know
- `docker ps`: Shows running/stopped Docker images
- `docker exec -it DOCKER_CONTAINER_ID /bin/bash`: opens new bash session inside specified container
- `airflow db init`: initiate metadata DB and create require files/folders for Airflow
- `airflow db upgrade`: upgrade Airflow (updates schema of metadata DB)
- `airflow db reset`: removes everything in database (TESTING ONLY)
- `airflow webserver`: start the webserver
- `airflow scheduler`: start airflow scheduler
- `airflow celery worker`: start celery worker
  - **Note:** Needs to be run on EVERY celery worker machine
- `airflow dags pause`: Pause DAGS
- `airflow dags unpause`: Unpause DAGs
- `airflow dags trigger [additional args for execution date, etc.]`: Trigger DAGs (with operational arguments added)
- `airflow dags list`: list all dags in a given initialized airflow directory
- `airflow tasks list DAG_ID`: List all tasks in a DAG
- `airflow tasks test DAG_ID TASK_ID [ARGS]`: Test specific task in a DAG
- `airflow dags backfill -s START_DATE_YYYY-MM-DD -e END_DATE_YYYY-MM-DD --reset_dagruns DAG_ID:` Backfill DAGs with additional optional args

### REST API

[Docs](https://airflow.apache.org/docs/apache-airflow/stable/stable-rest-api-ref.html)

## DAGs and Tasks

### DAG Skeleton
Use “with DAG(...) as dag” syntax for easier code writing. Also, choose a unique dag_id to make debugging easier.

### Demystifying DAG Scheduling

#### Parameters to know

- `start_date`: Date/time that DAG will begin to be scheduled (i.e. 1/1/2021 @ 10am PST)
- `schedule_interval`: Frequency at which DAG will run (i.e. every hour)
  - **Note: DAG is actually triggered at start_date + schedule_interval**
  - Based on above example, this DAG would be triggered at 1/1/2021 11:00 AM
- `execution_date`: Date/time that DAG runs every interval
  - Based on above example, this DAG would have execution_dates of 1/1/2021 10:00 AM, 1/1/2021 11:00 AM, 1/1/2021 12:00 PM, and so on
    - **Note: Even though the DAG will START at 1/1/2021 11:00 AM its first execution date is the start date of 1/1/2021 10:00 AM**
  - Every time DAG is executed, the start_date of the next DAG run is incremented by the schedule_interval
- `end_date`: Date/time that DAG will stop

### Playing with `start_date`

The `start_date` argument is added as an argument to an instantiated “DAG” object. The first upstream task will inherit this `start_time` value. Tasks *can* be set with different start_date values than the DAGs, but this will likely never come up in practice.

By default, all date times are in UTC time zone (Best practice).

**Note:** If DAG is created/triggered with start date in past (and DAG hasn’t been triggered yet), Airflow will trigger all untriggered DAG runs between start date and current date (assuming `catchup` is set to True; more on that below)

**Do NOT use dynamic datetimes like datetime.now() for the start_date. Every time the DAG evaluated, the start_date moves forward in time so the DAG will never be triggered.**

### Playing with `schedule_interval`

This argument is also added as an argument to an instantiated “DAG” object. This will tell Airflow how often this DAG should be run. 

This argument can either be a cron expression or timedelta object. A general rule of thumb is to use cron expressions when the DAG needs to be run every day at a certain time or certain days of the week. Use timedeltas where cron expressions fall short (i.e. when a DAG needs to be run every x minutes, hours or days).

More on cron expressions: https://crontab.guru/ 

### Backfilling and Catchup

- Backfilling: Running/rerunning past non-triggered or already triggered DAG runs
- Catchup: Boolean keyword argument added to “DAG” object to declare whether or not the DAG should do backfill (i.e. catchup=True for backfill)

Airflow also has a specialized Python method `days_ago` in the `airflow.utils.dates` library that can be used to declare a start date from x days ago (to be used only once + with catchup).

### Focus on Operators

- Operators can be thought of as a wrapper around a task; you should have 1 task per operator
  - i.e. if there are extraction and cleaning tasks, they should NOT be combined into the same operator
- Operators should have unique task ids
- Leverage the `default_args` argument in DAG objects to establish default values (i.e. `retry`, `retry_delay`) for all tasks within a DAG
  - I.e. create a `default_args` dictionary and pass into the `default_args` argument of an instantiated DAG object
- Look at [BaseOperator class](https://airflow.apache.org/docs/apache-airflow/stable/_api/airflow/models/baseoperator/index.html) to see all the different kinds of arguments that can be passed into most Operator classes
  - All other operators come from this one

### Executing Python Functions
The PythonOperator class is a specialized operator to execute Python code, specifically a Python callable function where one passed in  the name of the python function as an input to the Operator

**Neat trick:** One can access context information (i.e. DAG ID, task IDs, timestamps, etc.) of a DAG by
- Creating a python function that takes in keyword arguments (i.e. `**kwargs`) as it's only input
- Have the function print the keyword arguments (i.e. `print(kwargs)`)
- The result will be all of the DAG context information

**Another trick:** Pass in a dictionary of key value pairs into the `op_kwargs` argument of the PythonOperator and you’ll be able to access the values from the dictionary using the keys directly

### Putting your DAG on Hold
Sometimes one will want to pause a DAG in order to wait for specific conditions to be met (or to debug errors).

A common use case is to wait for a file to land in specific location before kicking off DAG. This use case (and similar ones where DAGs should wait to execute until conditions are met) can be solved by using Airflow "Sensor" objects. Specifically, a “FileSensor” object can solve the above example use case.
- Requires a connection id, which references a manually declared connection created in the Airflow UI
- Will need to manually add a connection in the Airflow UI to declare all needed info (i.e. AWS bucket creds)

### Executing Bash Commands
- Specialized Operator `BashOperator` that executes Bash commands
- Takes in bash command using the `bash_command` argument

### Define the path!
Airflow DAGs that have multiple tasks defined require relationships/dependencies between the tasks to be defined as well. This is because typical use cases for Airflow are typically (but not always) modeled in the form of ETL (Extract, Transform, Load) tasks; in general the later tasks typically need earlier tasks to complete before running.

Here are some best practices to specify relationships between tasks (i.e. declare dependencies between tasks)
- (Recommended) Use “bitshift” operators (<< or >>)
  - I.e. use `task1 >> task2` to have `task2` be downstream of (i.e. happen after) `task1`
- Use `set_upstream()` or `set_downstream()` functions
  - `set_upstream()` will make the task inside the function occur BEFORE the task that has set_upstream() called on it
    - I.e. `task2.set_upstream(task1)` will make task1 be an upstream task 2 (equivalent to `task2` being downstream of `task1`)

#### Additional examples:
- `task1 >> task2 >> task3`
  - This will have `task2` follow `task1` and `task3` follow `task2`
- `task1 >> [task2, task3]`
  - This will make both `task2` and `task3` follow `task1` in parallel
- The `chain()` function
  - `chain(task1, task2, task3)` is the same as `task1 >> task2 >> task3`
- The `cross_downstream()` function
  - `cross_downstream([task1, task2], [task3, task4])` will make `task3` and `task4` happen after `task1` and `task2`
- CANNOT do `[task1, task2] >> [task3, task4]`; cannot declare dependencies like that, must use `cross_downstream()` function

### Exchanging Data
Airflow has methodology for sharing data between tasks (with limitations). Airflow’s method for doing this is called Xcoms. To make values shareable:
- Method 1: have Python functions return values/objects you want to pass between tasks (easiest method)
  - Use if the name of the key does not matter; the key will be called `return_value`
- Method 2: return values/objects using `xcom_push()`, retreive values/objects using `xcom_pull()`
  - Use if you want to customize the name of the key
  - Example: in the task `task1`, the line of code `ti.xcom_push(key=’KEY_NAME’, value=VALUE)` is used
    - `ti` is the task instance object and must be passed into the Python function where the values/objects are to be pushed 
    - Will be able to see returned values in the Airflow UI after DAGs executed
  - To retrieve values from other tasks/Python functions: use `xcom_pull()`
  - Need to access context of DAG run, specifically the task instance object and pass the object into the Python function
    - If above object is `ti` and we are grabbing the value pushed from `task1` above: 
      - `my_xcom = ti.xcom_pull(key=’KEY_NAME’, task_ids=[‘task1’])`

### Ops...We got a Failure
Quick notes on notifications based on task/DAG states.

If you want to notify someone of:
- failed DAGs: add `email_on_failure` argument to default args list
- retried DAGs: add `email_on_retry` argument to default args list
  - Can add `on_failure_callback` parameter to Operators to call specific code in the event of failure
- Can get DAG context via variable called context???

This module also included typical debugging stuff; just make sure to navigate to the logs of failed tasks as needed.

## The Executor Kingdom

### The Default Executor
Airflow has different types of executors for specific types of needs which each define how tasks are executed (executing tasks in parallel, one by one, etc.). Behind the executors is a queue; the executor defines what system executes the tasks.

The default executor is Sequential executor, which triggers tasks one by one in order based on the defined dependencies. This executor uses SQLite as its metadata DB which doesn’t allow multiple writes at the same time (meaning tasks cannot be executed in parallel).

### Concurrency, the parameters you must know!
- `parallelism`: determined maximum number of tasks that can be executed at the same time in entire Airflow instance
- `dag_concurrency`: defines number of tasks that can be executed for a given task across all DAG runs
  - Default: 16 (i.e. can execute at most 16 tasks at same time for given DAG across all DAG runs)
- `concurrency`: parameter that does the same as the above parameter but can be set for specific DAGS that overrides `dag_concurrency`
- `max_active_runs_per_dag`: Defined maximum number of DAG runs that can be run at the same time
- `max_active_runs`: parameter that does the same as the above parameter but can be set for specific DAGS that overrides `max_active_runs_per_dag`

Example: if `parallelism` = 4, `dag_concurrency` = 16, and `max_active_runs_per_dag` = 16 the maximum number of concurrent tasks that can be run at the same time is 4 (`parallelism` overrides all else)

### Start Scaling Apache Airflow
If tasks needs to be executed in parallel but there's only a local machine available for use, look into using `LocalExecutor` as well as using a different DB for the metadata DB (i.e. PostGres DB).

### Scaling to the Infinity!
`LocalExecutor` is a good start for executing tasks in parallel but is still limited since tasks only run on local machine. Scaling this up is not sustainable; for many parallel tasks, a good option to look into is the **Celery Executor**. This executes tasks on multiple machines rather than one local machine.

**Note:** In order to properly do this, we must implement a queue system using 3rd party tool like Redis or RabbitMQ so that the queue can be executed on a different machine from the executor
**Note:** Also must set up a result backend!!!

A quick example architecture:
- Node 1: Web Server + Scheduler
- Node 2: Metadata DB (i.e. Postgres) + Queue
- Node 3: Celery worker
- Node 4: Celery worker
- Node 5: Celery worker 

**Note:** In order to initialize the celery workers for use with Airflow, the following steps must be taken:
1. Airflow must be installed on all machines as well as all required dependencies (Python packages, airflow provider packages, etc.)
2. One must run `airflow celery worker` directly on all celery worker machines (in the above example nodes 3-5)

When tasks are ready to go, scheduler (node 1) pushes task info to queue (node 2), then task is pushed to one of the celery workers (node 3/4/5).