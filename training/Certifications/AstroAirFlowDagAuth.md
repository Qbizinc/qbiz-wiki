---
title: Astronomer DAG Authoring for Apache Airflow
description: 
published: true
date: 2022-04-18T16:22:41.527Z
tags: 
editor: markdown
dateCreated: 2021-09-13T16:48:50.416Z
---

# Astronomer DAG Authoring for Apache Airflow
This article will help you as a study guide for the certification exam. However, I might miss some important remarks or exceptions made by the author. Feel free to contact me if something should be added

**IMPORTANT: Please finish the course before going through this guide**

## DAG Basics

### Dag Operator

The scheduler is only going to Parse a python file in the dag directory if the file has the word DAG or the word airflow, you can change this in the configuration file. 

Parameters of the DAG Constructor:

- Dag_id: Name of your dag ( if two DAGs have the same dag id no errors will show, one of the DAGs will appear randomly at every dag_dir_list_interval on the UI)
- Description
- start_date:  The date when your dag starts being scheduled
- schedule_interval: Defines the frequency the dag is triggered (None means the DAG must be triggered manually)
- dagrun_timeout: The time that you dag is going to run before timingout
- tags
- catchup: Prevents backfilling your dag runs

### Dag Scheduling

Remember, the dag starts being scheduled from the start_date and will be triggered after every schedule_inteval. This means that if you start_date is 10:00 AM and your schedule_interval is 10 minutes, your first dag run is at 10:10 AM

Execution_date: Is the logical date and time at which the dag and its task instances run. This also acts as a unique identifier for each DAG Run.

**EXTRA INFO:**
start_date = The first dag start time. keep it STATIC
execution_date = max(start_date, last_run_date)
schedule_interval parameter accepts cron or timedelta values
next_dag_start_date = execution_date + schedule_interval

schedule_interval parameter accepts cron or timedelta values. This initiates the next dag run by utilizing the formula
next_dag_start_date = max(start_date, last_run_date) + schedule_interval

Example: - if your start_date = datetime(2019, 10, 13, 15, 50), schedule_interval = 0 * * * * or (@hourly)

Case a) current_time is before start_date - 2019-10-13 00:00, then your dags will schedule at
2019-10-13 16:50, and subsequently every hour.
Please note that it will not start at start_date(2019-10-13 15:50), but rather at execution_date + schedule_interval

Case b) current_time is after start_date - 2019-10-14 00:00, then your dags will schedule at
2019-10-13 16:50, 2019-10-13 17:50, 2019-10-13 18:50 … and subsequently catchup till it reaches 2019-10-13 23:50
Then it will wait for the strike of 2019-10-14 00:50 for the next run.
Please note that the catchup can be avoided by setting catchup=False in dag object properties

### Cron vs Timedelta

In the schedule_interval parameter you can use Cron or a timedelta object, the difference between those is that Cron expression is stateless, whereas a timedelta is relative to the previous execution_date

### Idempotent and Deterministic
Every Dag should be Idempotent and Deteministic

- Idempotent: If you execute your dag multimple times, you should always get the same side effects
- Deterministic: If you execute your dag with the same inputs you will get the same output

### Backfilling
You can use the CLI (with the "airflow" command) or the UI to backfill your dags if necessary, indicating the start_date and end_date that you want to backfill. You can also clear the states of the dag runs in case you ran the dag before

- max_active_run (DAG object parameter): Allows you to select how many dag runs can be executed concurrently  (usefull for backfilling)

## Variables
Variables are a key-value object that is stored in the metadeta database in airflow. Dags can access this variables using the function Variable.get(*key_of_variable*). Variables are usefull when different dags interact with the same API endipoint or the same entry bucket. 

To create a variable you can use the UI, the REST API or the CLI. You can also hide variables from users using the "_secret" suffix

### Properly Fetch your Variables
Do not fecth variables outside of tasks as each time you dag is parsed, you will create a connection in the metedata database, even if you dag is not running yet. 

If you need to retrieve different variables for the same dags; to avoid connecting multiple times to the metadata database, you can create a variable with a json value. To fetch this type of variables you can use two methods:

- Variable.get(key_of_variable, deserialize_json=True)
- {{var.json.key_of_variable.key_of_json}} (Jinja Template)

### Environment Variables
If you need to completely hide a variable from your users, you can fetch an Envorimental Variable using the same methods as before, the name of the variables needs to have the sructure AIRFLOW_VAR_NAME='variable'. 

This process also removes the need of connecting to the metadata database

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

To simplify your code, you can allways put your grouped task in another folder inside the dag directory and import the tasks in your dag. You can also create nested TaskGroups (Groups inside another Group).

## Advanced Concepts
### Dynamic Tasks
You can create Dynamic tasks storing the values in a dictionary in your dag. You can only do this if the values are already known, airflow need to know in advance the strucutre of your dag.

### Branching Operator
With Branching you can execute tasks according to a condition; if your condition is True, then you have to return the task_id or multple task_id's that you want to execute in that condition. 

Remember that you have to allways return something, if you don't do that the dag will throw an error. You can solve this modifying your scheduing_interval or adding a Dummy/Stop task to the conditions that you need to fill

Examples of Branch Operators:

- BranchSQLOperator
- BranchPythonOperator

### Trigger Rules

A trigger rule defines why your task is going to be triggered. Examples of trigger rules:

- trigger_rule='all_success': (Default Value) The task gets triggered if all of his parents have succeeded
- trigger_rule='all_failed': The task gets triggered if all of his parents have failed
- trigger_rule='all_done': The task gets triggered if all of his parents have been triggered and completed. It doesn't care about the status of the parent tasks
- trigger_rule='one_success': The task gets triggered as soon as one of the parents succeeded
- trigger_rule='one_failed'
- trigger_rule='none_failed': The task gets triggered if all of the parents have succeeded or have been skipped
- trigger_rule='none_skipped': The task gets triggered if none of the parents have been skipped
- trigger_rule='none_failed_or_skipped': The task gets triggered if at least one the parents have succeeded and all of the parents have been completed
- trigger_rule='dummy': Your task gets triggered

### Dependencies and Helpers

If t1, t2, t3, t4, t5 and t6 are tasks, you can create dependencies in the following way:

- t2.set_upstream(t1)
- t2.set_downstream(t1)
- t1 >> t2
- [ t1, t2, t3 ] >> t4
- cross_downstream([t1, t2, t3], [t4, t5, t6]) (cross dependency, this doesn't work [t1, t2, t3] >> [t4, t5, t6])
- chain(t1, [t2, t3], [t4, t5], t6) (lenght of the lists have to be the same when chaining tasks)

### Concurrency

In the configuration file you can change the following variables to change the concurrency at the airflow level

- parallelism: Number of tasks that can be executed at the same time in your airflow instance, default value 32
- dag_concurrency: Number of tasks that can be executed at the same time for a given dag, default value 16
- max_active_run_per_dag: Number of dag runs that can be executed at the same time for a given dag, default value 16

At that dag level, you can add two parameters to your dag operator to modify the concurrency

- concurrency: The Airflow scheduler will run no more than this number of task instances for your DAG at any given time
- max_active_runs: Number of dags runs at the same time

At the task level

- task_concurrency:  This parameter controls the number of concurrent running task instances across dag_runs per task.

### Pools

Pools allow you to limit parallelism for an arbitrary set of tasks, giving you fine grained control over when your tasks are run. When creating a pool, you have set the number of worker slots that get taken by running tasks to limit execution parallelism

By default, all tasks in Airflow get assigned to the default_pool which has 128 slots. You can modify this value, but you can't remove the default pool. Tasks can be assigned to any other pool by updating the pool parameter. This parameter is part of the BaseOperator, so it can be used with any operator

Example:

Limit the concurrency of "process" tasks by creating a pool with 1 slot, so that they are executed sequentially while other task are executed simultaneously

When working with pools, there are a couple of limitations to keep in mind:

- Each task can only be assigned to a single pool.
- If you are using SubDAGs, pools must be applied directly on tasks inside the SubDAG. Pools set within the SubDAGOperator will not be honored.
- Pools are meant to control parallelism for Task Instances. If instead you are looking to place limits on the number of concurrent DagRuns for a single DAG or all DAGs, check out the max_active_runs and core.max_active_runs_per_dag parameters respectively.

### Task Priority

If there is a task that is critical in your pipeline and you want to run it first before the other tasks you can use the paramater priority_weight. 

When tasks are assigned to a pool, they will be scheduled as normal until all of the pool's slots are filled. As slots open up, the remaining queued tasks will start running. You can control which tasks within the pool are run first by assigning priority weights which define priorities for the executor queue. These are assigned at the pool level with the priority_weights parameter. The values can be any arbitrary integer (default is 1), and higher values get higher priority.

### Depends on past and wait for downstream

If you have set depends_on_past=True, if the task extract didn't succeed in the previous DAG Run, then it doesn't get triggered in the current DAG Run (except if it is the first run for that task). Also, if wait_for_downstream=True, all tasks immediately downstream of the previous task instance must have succeeded

### Sensors

Sensors are a special kind of operator. When they run, they will check to see if a certain criteria is met before they complete and let their downstream tasks execute. This is a great way to have portions of your DAG wait on some external system

- soft_fail: Set to true to mark the task as SKIPPED on failure
- poke_interval: Time in seconds that the job should wait in between each try. The poke interval should be more than one minute to prevent too much load on the scheduler.
- timeout: Time, in seconds before the task times out and fails. If you don't set a timeout, it will timeout after 7 days
- mode: How the sensor operates. Options are: { poke | reschedule }, default is poke. When set to poke the sensor will take up a worker slot for its whole execution time (even between pokes). Use this mode if the expected runtime of the sensor is short or if a short poke interval is required. When set to reschedule the sensor task frees the worker slot when the criteria is not met and it's rescheduled at a later time.
- execute_timeout: With timeout and soft_fail set to True, the Sensor will be marked as SKIPPED, whereas with execution_timeout it will be marked as FAILED

### Timeouts

You can define a timeout in the DAG object:

- dagrun_timeout: The time that you dag is going to run before timingout

At the operator level:

- execution_timeout: The time that you task is going to run before timingout

### Callback

A callback is nothing more than triggering a function according to an event, for example, if your task fails then you can trigger a callback function. The callback function can be a notification or print a message. You can define a callback at dag level or at task level

Example of callbacks:

- on_success_callback: Expects a python function
- on_failure_callback: Expects a python function
- on_retry_callback: Expects a python function

### Retries

Whenever a task fails you can modify how airflow is going to retry your task. To change this you can use the following parameters:

- retries: Number of retries before your task fails (Default 0). You can set the retries parameter at the task level, on the default arguments or at the airflow instance level. The retries task level is going to overwrite any other retries argument
- retry_delay: Defines the time that you want to wait between each retry
- retry_exponential_backoff: It will wait a little bit longer for each retry. Good for waiting for API's or waiting for databases
- max_retry_delay: Max time that is going to wait between each retry. Good with exponential_backoff

### SLA

The goal of an SLA is to verify that your task gets completed in period of time. Similar to callbacks, if the task is not completed in the expected time, you can trigger a Python function to get notified about this

An SLA is relative to the DAG execution date. At the dag level, if you want to trigger an sla, you can use the folling parameter:

- sla_miss_callback: _sla_miss_callback

Where _sla_miss_callback is a python function. This python function has the following parameters:

- dag
- task_list
- blocking_task_list
- slas
- blocking_tis

### DAG Versioning

The best practice is to manage the versioning of the dag in the dag_id. For example, you can name your dag "process_dag_1_0_1"

If you have a DAG running (version 1) with three tasks:

Task A is completed
Task B and C are queued

In the meantime, you deploy a new version of the DAG (version 2) with a code change to tasks B and C. The version that the queued tasks be based on is the second

Also, remember when you change the scheduling_interval of a DAG, previously TaskInstances won't align with the new schedule interval

### Dynamic DAGS

[More:] https://academy.astronomer.io/astronomer-certification-apache-airflow-dag-authoring-preparation/899677

## DAG Dependencies

### ExternalTaskSensor

A sensor that waits for an execution of a task in a different dag (external dag)

### TriggerDagRunOperator

Helpful if you want to trigger DAG A from DAG B. You can wait for the DAG triggered by the TriggerDagRunOperator to complete before executing the next task using "wait_for_completion"
