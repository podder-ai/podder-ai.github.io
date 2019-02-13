# Podder CLI
Podder CLI (podder-cli) is a Python-based command line tool to interact with Podder.ai from your terminal.

## Install Podder CLI
[podder-cli](https://pypi.org/project/podder-cli/) is available on pypi and runs on Python 3.5, and Python 3.6. podder-cli works on Windows, MacOS, and Linux.. You can also see the [Podder CLI](https://github.com/podder-ai/podder-cli) repository on GitHub.

Use pip to install the CLI.
```
$ pip install -U podder-cli
```


After installation you can view the commands supported by the CLI using the --help option.
```
$ podder --help
```


## Command
Below are the commands that are part of Podder CLI.

|Command|Description|
|:---|:---|
|task add|Add new task at the current path.|
|task build |Build task at the current path.|
|task obfuscate|Obfuscate task at the current path.|
|task pre-commit|Run formaters, linters and unit testsat the current path.|
|task run|Run task at the current path.|
|task tag|Tag task at the current path.|
|pipeline add|Add new pipeline at the current path.|
|pipeline compose|Build pipelines from yaml at the current path.|
|pipeline serve|Serve pipelines at the current path.|
|pipeline tag|Tag pipeline at the current path.|
|deployment build|Build kubernetes yaml file at the current path.|
|deploy|Deploy pipelines at the current path.|
|upgrade|Upgrade CLI to the latest version.|
|version|View the current version of the CLI.|

Run `podder COMMAND --help` for more information on a command.
