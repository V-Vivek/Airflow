## Tasks
- They are instances of an operator
- Tasks are individual units of work within a DAG. 
- Each task represents a single operation to be performed as part of a larger workflow.
- Tasks can be of various types, such as BashOperator, PythonOperator, or even custom operators that are built specifically for a particular use case. 
- Each task is associated with a unique task_id within the DAG and is executed by an executor as part of the workflow.

## Task dependencies
- Task dependencies are used to specify the order in which tasks should be executed within a DAG.
- Task dependencies allow you to define which tasks should be run first, which tasks can be run in parallel and which tasks should be executed only after certain other tasks have completed successfully.
- Task dependencies are defined using the set_upstream() and set_downstream() methods, or the >> and << operators, which can be used to set dependencies between tasks.

## Example
- Here, we're specifying that task2 and task3 depend on task1, meaning that task1 must complete successfully before either task2 or task3 can be executed.
```python
from airflow import DAG
from airflow.operators.bash import BashOperator
from datetime import datetime

# Define the DAG
dag = DAG(
    'my_dag',
    description='An example DAG with task dependencies',
    schedule_interval=None,
    start_date=datetime(2023, 4, 16),
    catchup=False
)

# Define the tasks
task1 = BashOperator(
    task_id='task1',
    bash_command='echo "Task 1"',
    dag=dag
)

task2 = BashOperator(
    task_id='task2',
    bash_command='echo "Task 2"',
    dag=dag
)

task3 = BashOperator(
    task_id='task3',
    bash_command='echo "Task 3"',
    dag=dag
)

# Set the task dependencies
task1 >> task2  # or task2 << task1
task1 >> task3  # or task3 << task1
```

- Note that we can also use the set_upstream() and set_downstream() methods to set dependencies between tasks
```python
task2.set_upstream(task1)
task3.set_upstream(task1)
```
