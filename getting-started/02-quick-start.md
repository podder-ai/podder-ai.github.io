# Quick Start
Let's serve our first AI module (pipeline) on your local machine! Follow this guide for a quick look at how easy it is to use Podder.ai.

## Quick Preparation Checklist
- Create a [Docker](https://www.docker.com/) account
- Create a [GitHub](https://github.com/) account
- Install [Podder CLI](https://pypi.org/project/podder-cli/) on your computer

## Get a new pipeline and tasks
In your terminal, use `podder-cli` to download a template of your task.
```
$ podder init
$ What is the name of your pipeline?: first-pipeline
$ What is the name of you task?: first-task
$ ls
first-pipeline    first-task
```

Now, you can see your first pipeline and task in your terminal. Here's what Podder.ai did when you ran that command.
1. Download a template of pipeline
2. Download a template of task
3. Update `pipeline.yml` in your pipeline project to define how to orchestrate your tasks
4. Build pipeline based on `pipeline.yml`

## Serve your first pipeline
In your terminal, use `podder-cli` to serve pipeline on Airflow Platform.
```
$ cd first-pipeline
$ podder pipeline serve
```

This command build docker image of your pipeline and task, then serve your pipeline on Airflow. It takes time when you first serve your pipeline.

## Run your first pipeline
You can strat your pipeline by turning on the button of your DAG (Pipeline is called DAG in Airflow).
Congratulations! You've executed your first pipleine on Podder.ai.

Yeah, that was pretty high level. To go a bit more in depth and learn more about what you actually just did, check out the Getting Started Tutorials
