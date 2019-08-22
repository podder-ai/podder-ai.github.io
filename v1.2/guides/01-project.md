# Project
On Podder.ai, a project is a collection of all the tasks and setting files you perform when developing an AI application.

## Create Podder Project
You can create a new podder project by the follwoing command.
```bash
$ podder init project-name
```

You can now see some new direcories are created under your project.
```bash
$ cd project-name
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