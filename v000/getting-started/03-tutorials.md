# Tutorials
With this tutorial, you'll get acquainted with Podder CLI/Web and learn the basics necessary to get up and running on your platform.

We'll start with an creating a new task, and then jump into building your first pipelines on Podder Web. Also, You will deploy the pipelines to Kubernetes Cluster on your local.

Here is the list of senarios we provide.
- Create Tasks
- Create Pipelines
- Deploy Pipelines

## Quick Preparation CheckList
- Create a [Docker](https://www.docker.com/) account
- Create a [GitHub](https://github.com/) account
- Install [Podder CLI](https://pypi.org/project/podder-cli/) on your computer
 
## Create Tasks
In this senario, we are going to create a new task and run on your local.

### Get a new task
First, create a new project directry. Run the following command on your terminal.
```
$ mkdir tutorials
```

Then, use `podder-cli` to download a template of your task.
```
$ podder task add logistic-regression
$ cd logistic-regression
```

### Write your algorithms
Open `logistic-regression` directory on your favarite editor. You can see the logistic regression algorithm in `task.py` under `app` directory. In Podder.ai, you need to write down your algorithms inside `execute` method of `task.py` file. It will get the privious task outputs as inputs in parameters, and carrying over your outputs to the next task inputs.

### Build a task
In your terminal, use `podder-cli` to build docker image of your task. It takes time when you first build your task.
```
$ podder task build
```

### Run a task
Then, to run the task on your local, we will use the following command.
```
$ podder task run
```

Now, you can see your task's logs in your terminal. Congraturation! You just created and ran your first task!


## Create pipelines
In this senario, we are going to create a new pipeline and combines multiple tasks we prepared. Then, we will serve your first pipeline on Airflow.

### Get a new pipeline
In your terminal, use `podder-cli` to download a template of your pupeline.
```
$ podder pipeline add tutorial-pipeline
$ cd tutorial-pipeline
```

### Build your first pipeline
Now, we are going to define a collection of tasks and their run order on `pipeline.yml` in tutorial-pipeline project.

#### Add new tasks
First, you need to download tasks which will be built into your pipeline. Run the following command in your project directory (under tutorial directory).

```
$ podder task add sensor-task
$ podder task add writer-task
```

sensor-task is a task to collect and manage throttling your input datasets. sensor-task does the following:
- Collect input datasets from the inside of sensor-task data directory
- Manage input datasets whether they are already executed or not
- Throttling the number of datasets to be executed in one pipeline running
- Copy new datasets to shared directory from data directory
- Pass the information of the datasets path as parameters to next tasks

writer-task is a task to output the result of your tasks to your local volume. writer-task does the following:
- Get the output data of your tasks as inputs parameters
- Output them into output directory under shared folder as json file
- Update the status of datasets to DONE!

We are strongly recommend you to use these sensor and writer tasks prepared by Podder.ai. It supports variaous storages and you can easily configure where to find input datasets and where to output the results of your AI modules.

#### Define tasks
Now, you are ready to define a coollection of tasks and their run order on `pipeline.yml`. Open the file under the `tutorial-pipeline` project on your editor, and modify it as following.
```
version: 1.0
dags:
  - dag_name: tutorial_pipeline
    schedule: *****
    flows:
      - - sensor-task
        - logistic-regression
        - writer-task
```

This `pipeline.yml` expects you to define the following things.
- The name of your pipelin (It is called DAG on Airflow, and you can have the same understanding of pipeline)
- Scheduling your pipeline
- A collection of your tasks and their run order

As you can see, you can configure multiple pipelines and flows in your pipelines. Please refere `Advanced Pipeline Setting` for more information.

#### Build pipelines
Finally, you can build & serve your first pipeline by using `podder-cli` command on your tarminal. Podder.ai will automatically configure all setting to serve your pipeline on Airflow.
```
$ cd tutorial-pipeline
$ podder pipeline build
```


### Serve Airflow
Let's start your pipline by using `podder-cli`! Run the following command to serve your pipeline.
```
$ podder pipeline serve
```

After your pipeline is served, you can access to `http://localhost:8080/admin` to open dashboard of Airflow. (It takes time to serve when you first build docker image of your pipeline). To start your pipeline, you can just turn it on by clicking the switch button on your DAG.

## Deploy pipelines
In this senario, we are going to deploy your pipeline on Kubernetes cluster on your local. This scenaio helps you to understand the basic flow of deployment of your AI modules.

TBC ...