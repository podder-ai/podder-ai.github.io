# Pipeline
A pipeline is a set of rules for defining a collection of tasks and their run order. Pipelines support complex job orchestration using a simple set of configuration keys to help you resolve failures sooner. (We are using [Airflow](https://airflow.apache.org/) to schedule and monitor pipelines.)

With Pipelines, you can:
- Run and troubleshoot tasks independently with real-time status feedback.
- Schedule pipeliens for tasks that should only run periodically.
- Run multiple tasks in parallel for efficient version testing.


## Configuration
You can define a collection of tasks and their run order in `config/pipeline.yml`.
```yaml
version: 1.0
tasks:                                  # Add your tasks here
  - task_name: sensor-task              # task name
    timeout: 45                         # timeout setting of task 
  - task_name: main-task
    timeout: 45
  - task_name: writer-task
    timeout: 45
dags:
  - dag_name: pipeline                  # your dag name
    schedule_interval: "* * * * *"      # modify this if you want to change schedule interval. (Cron expression)
    flows:                              # run order of tasks
      - - sensor-task
        - main-task
        - writer-task
task_log_format: "time:[%(time)s]\tname:%(name)s\ttaskname:%(taskname)s\tscriptinfo:[%(scriptinfo)s]\tloglevel:%(levelname)s\tprogresstime:%(progresstime)s\ttasktime:%(tasktime)s\tmessage:[%(message)s]"
task_log_level: DEBUG
sql_log_format: "time:[%(asctime)s]\tname:%(name)s\tloglevel:%(levelname)s\tmessage:[%(message)s]"
sql_log_level: WARN
```

## Start pipeline
You can start your pipeline by the following command.
```bash
$ podder pipelien start
```
Podder CLI read the configuration on pipeline.yml and start your pipeline.

TBD cannot execute podder pipeline ...