# Run Your First Application
## What Your Learn
Design the most important pipeline to use Podder.ai. By the end of this tutorial, you will be able to design a pipeline based on your AI model to run your application.
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
$ tree . L2
.
├── config                      # You can put config files here
│   ├── env
│   │   └── development.env     # Environment variables for development
│   └── pipeline.yml
├── error                           
├── input                       # Put your input data here
├── output                      # Output data is stored here
└── tmp                         # Temporary working files are stored here
```
The Podder.ai configuration file is collected in the config folder, and the pipeline configuration file is also located here.

## Add New Task
To build pipeline, you need to add a new task by the follwoing command. (We will add the template tasks provided from us here.)
```bash
$ podder task add sensor-task --template sensor
$ podder task add podder-task
$ podder task add writer-task --template writer
```
template tasks are donwloaded to your project directory.

By default settings, sensor-task and writer-task handle data in the input and output directories.
- sensor-task: Task to get input data
- podder-task: Task to execute your source code
- writer-task: task to output result
> When actually running the application, you can change the input/output destination by changing the settings of sensor-task and writer-task.

## Implement Task
Now we will implement some logic on the task. We will code the actual processing in `podder-task/app/task.py`. (The default is not processing anything.)

You can modify `execute` method of task.py. (If you would like to keep the current code, please move on to next step.)
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
You can see `output_data` is logged on your terminal.
> Data in `podder-task/tests/files/inputs.json` is used as input data. You can modify if you would like to test use cases of diffrent input data.

## Design Pipeline
Next let's build a pipeline using this task. The pipeline design is described in the YAML file. Check out `config/pipeline.yml`.
```yaml
version: 1.0
tasks:                                  # Add your tasks here
  - task_name: sensor-task              # task name
    timeout: 30                         # timeout setting of task 
  - task_name: podder-task
    timeout: 120
  - task_name: writer-task
    timeout: 30
dags:
  - dag_name: pipeline                  # your dag name
    schedule_interval: "* * * * *"      # modify this if you want to change schedule interval. (Cron expression)
    flows:                              # run order of tasks
      - - sensor-task
        - podder-task
        - writer-task
```
We do not need to add new tasks or change the execution order this time, so we will use the configuration file as it is. The application is launched based on the design defined in `pipeline.yml`.


## Start Application
Let's run the application using the pipeline we built. Enter the following command to launch the application.
```
$ podder pipeline start
```
> The first run takes some time as the build process runs.

When the application starts up, please access [http://localhost:8080](http://localhost:8080). You can check the dashboard and see your dag defined in `pipeline.yml`.

You can start your dag from the screen. After waiting for a few minutes, you can see the job is completed.

This is the end of the tutorial. You now understand how to use Podder CLI and design a pipeline using Podder.ai.


