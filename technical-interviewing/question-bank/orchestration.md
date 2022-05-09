---
title: Orchestration
description: 
published: true
date: 2022-05-09T22:48:11.216Z
tags: 
editor: markdown
dateCreated: 2022-05-09T22:47:07.284Z
---

# Interview Question: Orchestration
> **Language: Python3**
{.is-warning}

> **Difficulty: Medium**
{.is-success}

## Steps

1. Setup
2. Questions

## 1. Setup

1. Reset the Terminal if necessary. Make sure the IDE pane is set to Python3.
2. Set up the Python environment.
   1. Create `orchestration.py` inside of `~/python/` to make `import orchestration` work.
   2. Test the setup.

### 1.2. Set Up the Python Environment

#### 1.2.1. Create `orchestration.py`

1. Copy the following code block. The entire block is a shell command that will create the `~/python/orchestration.py` file.
2. Paste into the `Terminal` pane.
3. Press `Enter` to execute the command in the `Terminal` pane to execute the shell command.

**NB: Clicking or otherwise activating the `Reset terminal` command in the CodingView interview will delete this file.**

```
cat > ~/python/orchestration.py <<- EOF
import collections.abc
import enum


class DAG(object):
    dag_bag = {}

    @classmethod
    def init_dag_bag(cls):
        cls.dag_bag = {}

    def __init__(self, name):
        if name in self.dag_bag:
            raise RuntimeError()
        super().__init__()
        self.name = name
        self.tasks = {}
        self.dag_bag[name] = self

    dag_context_stack = []

    def __enter__(self):
        self.dag_context_stack.append(self)

    def __exit__(self, type, value, traceback):
        if self._is_cyclic():
            raise RuntimeError('DAG contains cycle')
        self.dag_context_stack.pop()

    def add_task(self, task):
        if task.name in self.tasks:
            raise RuntimeError()
        self.tasks[task.name] = task

    def _is_cyclic(self):
        visited = set()
        recur_stack = set()

        def _is_cyclic_recur(task):
            visited.add(task.name)
            recur_stack.add(task.name)
            for downstream_task in task.downstream_tasks:
                if downstream_task.name not in visited:
                    if _is_cyclic_recur(downstream_task):
                        return True
                elif downstream_task.name in recur_stack:
                    return True
            recur_stack.remove(task.name)
            return False

        for task_name, task in self.tasks.items():
            if task_name not in visited:
                if _is_cyclic_recur(task):
                    return True
        return False


class TriggerRule(str, enum.Enum):
    ALL_SUCCESS = "all_success"
    ALL_FAILED = "all_failed"
    ALL_DONE = "all_done"
    ONE_SUCCESS = "one_success"
    ONE_FAILED = "one_failed"
    NONE_FAILED = "none_failed"
    NONE_SKIPPED = "none_skipped"

    def __str__(self) -> str:
        return self.value


class Operator(collections.abc.Hashable):
    def __init__(self, name, trigger_rule=TriggerRule.ALL_SUCCESS):
        super().__init__()
        self.name = name
        self.trigger_rule = trigger_rule
        self.dag = DAG.dag_context_stack[-1]
        self.dag.add_task(self)
        self.upstream_tasks = set()
        self.downstream_tasks = set()

    def __hash__(self):
        return hash(self.name)

    def __eq__(self, __o):
        return super().__eq__(__o)

    def __lshift__(self, __o):
        self.upstream_tasks.add(__o)
        __o.downstream_tasks.add(self)

    def __rshift__(self, __o):
        self.downstream_tasks.add(__o)
        __o.upstream_tasks.add(self)


class DagRunState(str, enum.Enum):
    QUEUED = "queued"
    RUNNING = "running"
    SUCCESS = "success"
    FAILED = "failed"

    def __str__(self) -> str:
        return self.value


class TaskInstanceState(str, enum.Enum):
    REMOVED = "removed"  # Task vanished from DAG before it ran
    SCHEDULED = "scheduled"  # Task should run and will be handed to executor soon

    # Set by the task instance itself
    QUEUED = "queued"  # Executor has enqueued the task
    RUNNING = "running"  # Task is executing
    SUCCESS = "success"  # Task completed
    SHUTDOWN = (
        "shutdown"  # External request to shut down (e.g. marked failed when running)
    )
    RESTARTING = "restarting"  # External request to restart (e.g. cleared when running)
    FAILED = "failed"  # Task errored out
    UP_FOR_RETRY = "up_for_retry"  # Task failed but has retries left
    UP_FOR_RESCHEDULE = "up_for_reschedule"  # A waiting reschedule sensor
    UPSTREAM_FAILED = "upstream_failed"  # One or more upstream deps failed
    SKIPPED = "skipped"  # Skipped by branching or some other mechanism
    SENSING = "sensing"  # Smart sensor offloaded to the sensor DAG
    DEFERRED = "deferred"  # Deferrable operator waiting on a trigger

    def __str__(self) -> str:
        return self.value


class State(object):
    # Backwards-compat constants for code that does not yet use the enum
    # These first three are shared by DagState and TaskState
    SUCCESS = TaskInstanceState.SUCCESS
    RUNNING = TaskInstanceState.RUNNING
    FAILED = TaskInstanceState.FAILED

    # These are TaskState only
    NONE = None
    REMOVED = TaskInstanceState.REMOVED
    SCHEDULED = TaskInstanceState.SCHEDULED
    QUEUED = TaskInstanceState.QUEUED
    SHUTDOWN = TaskInstanceState.SHUTDOWN
    RESTARTING = TaskInstanceState.RESTARTING
    UP_FOR_RETRY = TaskInstanceState.UP_FOR_RETRY
    UP_FOR_RESCHEDULE = TaskInstanceState.UP_FOR_RESCHEDULE
    UPSTREAM_FAILED = TaskInstanceState.UPSTREAM_FAILED
    SKIPPED = TaskInstanceState.SKIPPED
    SENSING = TaskInstanceState.SENSING
    DEFERRED = TaskInstanceState.DEFERRED

    task_states = (None,) + tuple(TaskInstanceState)

    dag_states = (
        DagRunState.QUEUED,
        DagRunState.SUCCESS,
        DagRunState.RUNNING,
        DagRunState.FAILED,
    )

    running = frozenset(
        [
            TaskInstanceState.RUNNING,
            TaskInstanceState.SENSING,
            TaskInstanceState.DEFERRED,
        ]
    )

    finished = frozenset(
        [
            TaskInstanceState.SUCCESS,
            TaskInstanceState.FAILED,
            TaskInstanceState.SKIPPED,
            TaskInstanceState.UPSTREAM_FAILED,
        ]
    )

    unfinished = frozenset(
        [
            None,
            TaskInstanceState.SCHEDULED,
            TaskInstanceState.QUEUED,
            TaskInstanceState.RUNNING,
            TaskInstanceState.SENSING,
            TaskInstanceState.SHUTDOWN,
            TaskInstanceState.RESTARTING,
            TaskInstanceState.UP_FOR_RETRY,
            TaskInstanceState.UP_FOR_RESCHEDULE,
            TaskInstanceState.DEFERRED,
        ]
    )

    failed_states = frozenset(
        [TaskInstanceState.FAILED, TaskInstanceState.UPSTREAM_FAILED]
    )

    success_states = frozenset([TaskInstanceState.SUCCESS, TaskInstanceState.SKIPPED])

    terminating_states = frozenset(
        [TaskInstanceState.SHUTDOWN, TaskInstanceState.RESTARTING]
    )


class Run(object):
    def __init__(self, fail_task_names):
        super().__init__()
        self.fail_task_names = fail_task_names
        self._events = []
        self._dag_states_by_name = {}
        self._task_states_by_names = {}
        self.run()
        self.events_by_key = {tuple(event): i for i, event in enumerate(self._events)}
    
    @property
    def states(self):
        out = ''
        for name, state in self._dag_states_by_name.items():
            out += str(('dag', name, state))
            out += '\n'
        for names, state in self._task_states_by_names.items():
            out += str(('task', names[0], names[1], state))
            out += '\n'
        return out

    @property
    def events(self):
        out = ''
        for event in self._events:
            out += str(event)
            out += '\n'
        return out

    def set_dag_state(self, dag_name, dag_run_state):
        self._events.append(("dag", dag_name, dag_run_state))
        self._dag_states_by_name[dag_name] = dag_run_state

    def set_task_state(self, dag_name, task_name, task_instance_state):
        self._events.append(("task", dag_name, task_name, task_instance_state))
        self._task_states_by_names[(dag_name, task_name)] = task_instance_state

    def dag_state(self, dag_name):
        return self._dag_states_by_name[dag_name]

    def task_state(self, dag_name, task_name):
        return self._task_states_by_names[(dag_name, task_name)]

    def _is_one_upstream_task_unfinished(self, dag_name, task):
        return any(
            self.task_state(dag_name, upstream_task.name) in State.unfinished
            for upstream_task in task.upstream_tasks
        )

    def _is_all_upstream_tasks_finished(self, dag_name, task):
        return all(
            self.task_state(dag_name, upstream_task.name) in State.finished
            for upstream_task in task.upstream_tasks
        )

    def _is_all_upstream_tasks_success(self, dag_name, task):
        return all(
            self.task_state(dag_name, upstream_task.name) == TaskInstanceState.SUCCESS
            for upstream_task in task.upstream_tasks
        )

    def _is_one_upstream_task_failed_states(self, dag_name, task):
        return any(
            self.task_state(dag_name, upstream_task.name) in State.failed_states
            for upstream_task in task.upstream_tasks
        )

    def _is_all_upstream_tasks_failed_states(self, dag_name, task):
        return all(
            self.task_state(dag_name, upstream_task.name) in State.failed_states
            for upstream_task in task.upstream_tasks
        )

    def _is_one_upstream_task_success(self, dag_name, task):
        return any(
            self.task_state(dag_name, upstream_task.name) == TaskInstanceState.SUCCESS
            for upstream_task in task.upstream_tasks
        )

    def _is_none_upstream_task_success(self, dag_name, task):
        return not self._is_one_upstream_task_success(dag_name, task)

    def _is_none_upstream_task_failed_states(self, dag_name, task):
        return not self._is_one_upstream_task_failed_states(dag_name, task)

    def _is_none_upstream_task_skipped(self, dag_name, task):
        return not any(
            self.task_state(dag_name, upstream_task.name) == TaskInstanceState.SKIPPED
            for upstream_task in task.upstream_tasks
        )

    def _is_all_tasks_finished(self, dag):
        return all(
            self.task_state(dag.name, task_name) in State.finished
            for task_name in dag.tasks.keys()
        )

    def _is_one_task_failed_states(self, dag):
        return any(
            self.task_state(dag.name, task_name) in State.failed_states
            for task_name in dag.tasks.keys()
        )

    def run(self):
        for dag_name, dag in DAG.dag_bag.items():
            self.set_dag_state(dag_name, DagRunState.QUEUED)
            self.set_dag_state(dag_name, DagRunState.RUNNING)
            for task_name in dag.tasks.keys():
                self.set_task_state(dag_name, task_name, TaskInstanceState.QUEUED)
        changed = True
        while changed:
            changed = False
            for (dag_name, task_name), task_state in self._task_states_by_names.items():
                dag = DAG.dag_bag[dag_name]
                task = dag.tasks[task_name]
                if task_state in State.finished:
                    continue
                elif task_state == TaskInstanceState.SCHEDULED:
                    self.set_task_state(dag_name, task_name, TaskInstanceState.RUNNING)
                    changed = True
                elif task_state == TaskInstanceState.RUNNING:
                    if (dag_name, task_name) in self.fail_task_names:
                        self.set_task_state(
                            dag_name, task_name, TaskInstanceState.FAILED
                        )
                        changed = True
                    else:
                        self.set_task_state(
                            dag_name, task_name, TaskInstanceState.SUCCESS
                        )
                        changed = True
                elif task_state == TaskInstanceState.QUEUED:
                    if not task.upstream_tasks:
                        self.set_task_state(
                            dag_name, task_name, TaskInstanceState.SCHEDULED
                        )
                        changed = True
                    elif self._is_one_upstream_task_unfinished(dag_name, task):
                        pass
                    elif self._is_all_upstream_tasks_finished(dag_name, task):
                        trigger_rule = (
                            DAG.dag_bag[dag_name].tasks[task_name].trigger_rule
                        )
                        if (
                            (
                                trigger_rule == TriggerRule.ALL_SUCCESS
                                and self._is_all_upstream_tasks_success(dag_name, task)
                            )
                            or (
                                trigger_rule == TriggerRule.ALL_FAILED
                                and self._is_all_upstream_tasks_failed_states(
                                    dag_name, task
                                )
                            )
                            or trigger_rule == TriggerRule.ALL_DONE
                            or (
                                trigger_rule == TriggerRule.ONE_SUCCESS
                                and self._is_one_upstream_task_success(
                                    dag_name, task
                                )
                            )
                            or (
                                trigger_rule == TriggerRule.ONE_FAILED
                                and self._is_one_upstream_task_failed_states(
                                    dag_name, task
                                )
                            )
                            or (
                                trigger_rule == TriggerRule.NONE_FAILED
                                and self._is_none_upstream_task_failed_states(
                                    dag_name, task
                                )
                            )
                            or (
                                trigger_rule == TriggerRule.NONE_SKIPPED
                                and self._is_none_upstream_task_skipped(dag_name, task)
                            )
                        ):
                            self.set_task_state(
                                dag_name, task_name, TaskInstanceState.SCHEDULED
                            )
                            changed = True
                        elif trigger_rule in (
                            TriggerRule.ALL_SUCCESS,
                            TriggerRule.ONE_SUCCESS,
                            TriggerRule.NONE_FAILED,
                            TriggerRule.NONE_SKIPPED,
                        ) and self._is_one_upstream_task_failed_states(dag_name, task):
                            self.set_task_state(
                                dag_name, task_name, TaskInstanceState.UPSTREAM_FAILED
                            )
                            changed = True
                        elif trigger_rule in (
                            TriggerRule.ALL_SUCCESS,
                            TriggerRule.ALL_FAILED,
                            TriggerRule.ONE_SUCCESS,
                            TriggerRule.ONE_FAILED,
                            TriggerRule.NONE_FAILED,
                            TriggerRule.NONE_SKIPPED,
                        ):
                            self.set_task_state(
                                dag_name, task_name, TaskInstanceState.SKIPPED
                            )
                            changed = True
                        else:
                            raise RuntimeError()
                    else:
                        raise RuntimeError()
            for dag_name, dag_state in self._dag_states_by_name.items():
                if dag_state in (DagRunState.SUCCESS, DagRunState.FAILED):
                    continue
                dag = DAG.dag_bag[dag_name]
                if not self._is_all_tasks_finished(dag):
                    continue
                if self._is_one_task_failed_states(dag):
                    self.set_dag_state(dag_name, DagRunState.FAILED)
                else:
                    self.set_dag_state(dag_name, DagRunState.SUCCESS)
EOF
```

## 1.2.2. Test the Setup

1. Copy the following code block.
2. Paste into the IDE pane.
3. Confirm the initial code works by clicking or activating `Run`.

```
import orchestration as o


o.DAG.init_dag_bag()

with o.DAG('d') as d:
  t0 = o.Operator('t')


run = o.Run([])

print('States:')
print(run.states)
print('Events:')
print(run.events)
```

## 2. Questions

### Question Text

This series of questions tests understanding of orchestration concepts:

* Task dependencies within a DAG.
* Task-level trigger rules based on upstream task instance states.
* Use of 4 task instance states to build logic flows:
  * not yet done
  * done w/o success or failed
  * success
  * failed

### Questions

1. Q1: With `trigger_rule`
2. Q2: XOR

## Tricks and Traps

TBD.

## Follow on questions

TBD.


### 2.1. Q1: With `trigger_rule`

1. Setup
2. Question
3. Solution

#### 2.1.1 Setup

1. Copy the following code block.
2. Paste into the IDE pane.
3. Confirm the initial code works by clicking or activating `Run`. The terminal will display a `True`, which is what the Python code prints.
4. Send diagram in email.
   From the `QBiz Shared Drive`: https://drive.google.com/file/d/11rCkRpjggHGPvSCIL1wPULIBenUTcPZs/view?usp=sharing
5. Talk through question:
   The section on the left is implemented in the code in the IDE pane. Complete the section on the right.

```
import orchestration as o


o.DAG.init_dag_bag()

with o.DAG('d') as d:
  t0 = o.Operator('insert')
  t1 = o.Operator('upload')
  t2 = o.Operator('insert_failed', trigger_rule='all_failed')
  t3 = o.Operator('upload_failed', trigger_rule='all_failed')
  t0 >> t2
  t1 >> t3
  t4 = o.Operator('all_success', trigger_rule='all_success')
  t0 >> t4
  t1 >> t4
  t5 = o.Operator('all_failed', trigger_rule='all_failed')
  t0 >> t5
  t1 >> t5

run = o.Run([])

print('States:')
print(run.states)
print('Events:')
print(run.events)
```

#### 2.1.2. Question

1. Debug with the following cases:
   1. All succeed: `run = o.Run([])`
   2. Insert failed: `run = o.Run([('d', 'insert')])`
   3. Upload failed: `run = o.Run([('d', 'upload')])`
   4. Both failed: `run = o.Run([('d', 'insert'), ('d', 'upload')])`
   5. Dashboard failed: `run = o.Run([('d', 'dashboard')])`
2. Talk through failure of notifications:
   1. Insert failed and insert failure notification failed: `run = o.Run([('d', 'insert'), ('d', 'insert_failed')])`

#### 2.1.3. Solution

```
import orchestration as o


o.DAG.init_dag_bag()

with o.DAG('d') as d:
  t0 = o.Operator('insert')
  t1 = o.Operator('upload')
  t2 = o.Operator('insert_failed', trigger_rule='all_failed')
  t3 = o.Operator('upload_failed', trigger_rule='all_failed')
  t0 >> t2
  t1 >> t3
  t4 = o.Operator('all_success', trigger_rule='all_success')
  t0 >> t4
  t1 >> t4
  t5 = o.Operator('all_failed', trigger_rule='all_failed')
  t0 >> t5
  t1 >> t5
  t6 = o.Operator('dashboard')
  t4 >> t6
  t7 = o.Operator('dashboard_success', trigger_rule='all_success')
  t6 >> t7
  t8 = o.Operator('dashboard_failed', trigger_rule='all_failed')
  t6 >> t8

run = o.Run([])

print('States:')
print(run.states)
print('Events:')
print(run.events)
```

### 2.2. Q2: XOR

1. Setup
2. Question
3. Solution

#### 2.2.1. Setup

1. Copy the following code block.
2. Paste into the IDE pane.
3. Confirm the initial code works by clicking or activating `Run`. The terminal will display a `True`, which is what the Python code prints.
4. Talk through question.

```
import orchestration as o


o.DAG.init_dag_bag()

with o.DAG('d') as d:
  t0 = o.Operator('insert_data_into_s3')
  t1 = o.Operator('insert_data_into_data_warehouse')
  t2 = o.Operator('send_all_success_email', trigger_rule='all_success')
  t0 >> t2
  t1 >> t2
  t3 = o.Operator('send_all_failed_text_message', trigger_rule='all_failed')
  t0 >> t3
  t1 >> t3
  t4 = o.Operator('send_one_failed_text_message', trigger_rule='one_failed')
  t0 >> t4
  t1 >> t4
  # t4 runs even when both t0 and t1 fail

run = o.Run([])

print('States:')
print(run.states)
print('Events:')
print(run.events)
```

#### 2.2.2. Question

TBD.

#### 2.2.3. Solution

```
import orchestration as o


o.DAG.init_dag_bag()

with o.DAG('d') as d:
  t0 = o.Operator('insert_data_into_s3')
  t1 = o.Operator('insert_data_into_data_warehouse')
  t2 = o.Operator('send_all_success_email', trigger_rule='all_success')
  t0 >> t2
  t1 >> t2
  t3 = o.Operator('send_all_failed_text_message', trigger_rule='all_failed')
  t0 >> t3
  t1 >> t3
  t4 = o.Operator('any_failed_do_nothing', trigger_rule='one_failed')
  t0 >> t4
  t1 >> t4
  t5 = o.Operator('any_success_do_nothing', trigger_rule='one_success')
  t0 >> t5
  t1 >> t5
  t6 = o.Operator('send_one_failed_text_message', trigger_rule='one_success')
  t4 >> t6
  t5 >> t6

run = o.Run([])

print('States:')
print(run.states)
print('Events:')
print(run.events)
```
