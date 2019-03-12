# Core Concepts
The essential points for understanding and effectively using Podder.ai can be grouped into the following categories:
- Tasks
- Pipelines

## Tasks
The task is the most important concept in Podder.ai. It is a unit of each steps in your AI modules and run on a single docker container. You are going to write codes on a task-template which will be added by Podder CLI, and push to your GitHub Repositry. You can build and deploy docker image from both of Podder Web and Podder CLI.

## Pipelines
A pipeline is a set of rules for defining a collection of tasks and their run order. Pipelines support complex job orchestration using a simple set of configuration keys to help you resolve failures sooner.
(Currently, we are using [Airflow](https://airflow.apache.org/) to schedule and monitor pipelines.)

With Pipelines, you can:
- Run and troubleshoot tasks independently with real-time status feedback.
- Schedule pipeliens for tasks that should only run periodically.
- Run multiple tasks in parallel for efficient version testing.

