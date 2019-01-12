# Core Concepts

The essential points for understanding and effectively using Podder.ai can be grouped into the following categories:
- Projects
- Tasks
- Pipelines
- Jobs
- Datasets
- Environments
- Output

## Projects
The project is the most central construct of the Podder.ai platform. It directly correlates with what you'd think of as a deep learning project outside of Podder.ai: You have a problem you need to solve with a deep learning model. You get on your computer, create a new directory with a name like mnist-cnn and boom, you've started a new project. In that directory, you'll write some code, run some experiments, and iterate until you have created a deep learning model that meets your needs.

## Tasks
The task is the most important concept in Podder.ai. It is a unit of each steps in your AI modules and run on a single docker container. You are going to write codes on a task-template which will be added by Podder CLI, and push to your GitHub Repositry. You can build and deploy docker image from both of Podder Web and Podder CLI.

## Pipelines
A pipeline is a set of rules for defining a collection of tasks and their run order. Pipelines support complex job orchestration using a simple set of configuration keys to help you resolve failures sooner.
(Currently, we are using [Airflow](https://airflow.apache.org/) to schedule and monitor pipelines.)

With Pipelines, you can:
- Run and troubleshoot tasks independently with real-time status feedback.
- Schedule pipeliens for tasks that should only run periodically.
- Run multiple tasks in parallel for efficient version testing.

## Jobs
A job is what pulls together your code and dataset(s), sends them to a server configured with the right environment, and actually kicks off the necessary code to get the data science done. After a job is completed, you'll be able to go back and reference/review it on Podder Web. For each job, you'll be able to see:

- The result of the job sccessfully finished or not
- A snapshot of the code used for the job
- A record of which Dataset(s) you used for the job
- A record of what Environment was used for the job
- The output and logs of the job
- The metrix of resouces and traning of the job

You run jobs using Podder CLI. The command has various flags and parameters that let you customize how the job runs: What job should be executed? What dataset do you want to use? Is the source code obfuscated when the task build? Any other commands you'd like to run on the server to set it up before running your job? Etc.


## Datasets
Podder Web's dataset workflow is one of two things that tend to feel a bit foreign to users who are used to local development (the other being output). When working on your local machine, you might have your dataset in the same directory as your code, or in a directory where you keep many different datasets. When using Podder Web, datasets are always kept separate from code.

### Why keep datasets separate from code?
As a data scientist you tweak your code often during the process of creating a deep-learning model. However, you don't change the underlying data nearly as often, if at all.

Each time you run a job on Podder Web, a copy of your code is uploaded to Podder Web and run on one of Podder Web's powerful deep-learning servers. Because your data isn't changing from job to job, it wouldn't make sense to keep your data with your code and upload it with each job. Instead, we upload a dataset once, and attach, or "mount", it to each job. This saves a significant amount of time on each job.

Beyond that, keeping data separate from code allows you to collaborate more easily with others. A dataset can be used by any user who has access to it, so teams and communities can work on solving problems together using the same underlying data.

### Connecting code and datasets
You'll still be writing your code locally when using Podder Web, but when you run a job, your code will be uploaded to Podder Web and executed on a powerful deep-learning server that has your datasets mounted to it.

You can specify the places where your datasets will be mounted on the server, but you'll have to make sure that your code references your datasets with the file paths of the data on the server, not where you might have them locally.


## Environments
Podder Web has a bunch of different deep learning environments to choose from. When you run a job on Podder Web, you'll be able to specify the environment you want use. You'll also be able to specify whether you want the job run using a GPU or a CPU.

If Podder Web's stock deep learning environments don't meet your needs, you can create a custom environment for your job. See this guide for instructions on that.

## Output
Output is anything from a job that you want to save for future use. The most common form of output is model checkpoints (the weights and biases of your model) that you developed during a job. If you save these checkpoints (or anything else you'd like to preserve) during a job, you'll have them to reference, download, and reuse in the future.

Output is the way that you can link jobs together: You run a job to test an idea you have. If it works, you may want to start where you left off and run another job. If going down that path leads to a dead end, you may want to go back to a previous output and start again from there. Knowing how to store output is key to optimizing your deep learning workflow.

Saving and reusing output on Podder Web can feel foreign to users who are used to working on their own machines. But once you learn how to do it, it becomes very simple and is one of the most valuable parts of the Podder Web workflow.