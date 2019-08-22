# Task
The task is the most important concept in Podder.ai. It is a unit of each steps in your AI modules and run on a single docker container. You can add your code to task.

## Add new task
You can add new task by the following command.
```bash
$ podder task add your-task-name
```

## How to implement
This is directory structure of task.
```bash
$ tree . -L 3
.
├── Dockerfile
├── README.md
├── app
│   └── task.py             # main task implementation
├── requirements.txt        # add required packages here
├── setup.cfg
└── tests
    ├── files
    │   └── inputs.json     # will be used as input data when you run your task
    └── unit
        └── app             # add unit test here
```

## Run task
You can run task by the following command.
```bash
$ podder task run your-task-name
```

`tests/files/inputs.json` is used as input data when you run your task. You can modify this json file if you want. 

## SSH task
You can ssh into docker container of task by the following command.
```bash
$ podder task ssh your-task-name
```

## Test task
You can run test of task by the following command.
```bash
$ podder task test your-task-name
```

[pytest](https://docs.pytest.org/en/latest/) is used for testing. Please check the official document for more infomation.
