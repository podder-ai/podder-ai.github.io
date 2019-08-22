# Run Your First Application
## What Your Learn
In this tutorial, you will design a pipeline and integrate your AI model, then you can run your first application equipped with your AI model.
- How to use Podder CLI
- Behavior of AI application using Podder.ai
> `podder-cli` is a command line interface for Podder.ai.

## Create Project
Let's create a project using the `podder init` command.
```bash
$ podder init sample-project
```

If you check the contents of the project, it has the following directory structure.
```bash
$ cd sample-project
.
├── config                      # You can put config files here
│   ├── env
│   │   └── development.env     # Environment variables for development
│   └── pipeline.yml            # Config file for pipeline
├── error                           
├── input                       # Put your input data here
├── output                      # Output data is stored here
└── tmp                         # Temporary working files are stored here
```
You need to put your AI model's config files under `config` folder.

## Add New Task
To build pipeline, you need to add a new task by the following command. (We will add the template tasks provided from us here.)
```bash
$ podder task add sensor-task --template sensor
$ podder task add podder-task
$ podder task add writer-task --template writer
```
template tasks are donwloaded to your project directory.

By default settings, sensor-task and writer-task will read the data in `input` directory and output the result into `output` directory.
- sensor-task: Task to collect input data
- podder-task: Task to execute your source code
- writer-task: task to output result

## Implement Task
Now, we will implement some logic on `podder-task`. We will write some codes in `podder-task/app/task.py`.

You can modify the code inside of `execute` method in task.py. (If you would like to keep the current code, please move on to next step.)
> DO NOT change the interface of execute method.

```
# app.task.py
・・・
@logger.class_logger
class Task(BaseTask):
    ・・・
    def execute(self, input_data: Any) -> Any:
        ・・・
        self.logger.debug("Start executing...")
        self.logger.debug("input_data: {}".format(input_data))

        # write your source code here
        output_data = input_data

        self.logger.debug("outputs: {}".format(output_data))
        self.logger.debug("Complete executing.")
        return output_data
```

Now that the task implementation is complete, let's see if it works and we expect it to work. Run the following command in the project directory.
```bash
$ podder task run podder-task
```
> It takes some time to build docker image when you first run your task.

You can see `Successfully run task.` if your task is completed successfully/
> Data in `podder-task/tests/files/inputs.json` is used as input data. You can add/update input files if you would like to test different data.

## Design Pipeline
Next, let's build a pipeline. The pipeline design is described in the YAML file. Check out `config/pipeline.yml`.
```yaml
version: 1.0
tasks:                                  # Add your tasks here
  - task_name: sensor-task              # Task name
    timeout: 30                         # Timeout setting of task
  - task_name: podder-task
    timeout: 120
  - task_name: writer-task
    timeout: 30
dags:
  - dag_name: pipeline                  # Your dag name
    schedule_interval: "* * * * *"      # Modify this if you want to change schedule interval. (Cron expression)
    max_active_runs: 1                  # The number of active dags to be running
    flows:                              # Running order of tasks
      - task_name: sensor-task
        downstream_tasks:
          - podder-task
      - task_name: podder-task
        downstream_tasks:
          - writer-task
      - task_name: writer-task
task_log_format: "time:[%(time)s]\tname:%(name)s\ttaskname:%(taskname)s\tscriptinfo:[%(scriptinfo)s]\tloglevel:%(levelname)s\tprogresstime:%(progresstime)s\ttasktime:%(tasktime)s\tmessage:[%(message)s]"
task_log_level: DEBUG
sql_log_format: "time:[%(asctime)s]\tname:%(name)s\tloglevel:%(levelname)s\tmessage:[%(message)s]"
sql_log_level: WARN
airflow_task_log_format: "time:[%(asctime)s]\tname:%(name)s\tloglevel:%(levelname)s\tmessage:[%(message)s]"
airflow_task_log_level: INFO
```
We do not need to add new tasks or change the execution order in this tutorial.


## Start Application
Let's run the application using the pipeline we built. Enter the following command to launch the application.
```
$ podder pipeline start
```
> It takes some time to build docker image when you first run your pipeline.

When the application starts up, please access to [http://localhost:8080](http://localhost:8080). You can check Airflow dashboard and see your dag defined in `pipeline.yml`.

You can copy input data to `input` directory by the following command.
```bash
$ cp podder-task/tests/files/inputs.json input/inputs.json
```
> In default setting, pipeline get input data from `input` directory.

You can activate your dag from the screen by switching your dag to `ON` status. After waiting for a few minutes, you can see the job is completed and the result is outputted to `output` directory.

This is the end of the tutorial. You now understand how to use Podder CLI and design a pipeline on Podder.ai.
