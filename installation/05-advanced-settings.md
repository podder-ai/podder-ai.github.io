# Advanced Settings
In this section, you will get the following things:
- [Change Sensor Task Setting](/installation/05-advanced-settings?id=change-sensor-task-setting)
- [Change Writer Task Setting](/installation/05-advanced-settings?id=change-writer-task-setting)

## Change Sensor Task Setting
[sensor-task](https://github.com/podder-ai/sensor-task) supports the following input storages.
- Local Storage
- Google Cloud Storage

### Get From Local Storage
You can change the environment variables to setup input storage. (See .env.examples)

|Key|Required|Example|Detail|
|:---|:---|:---|:---|
|INPUT_SOURCE|Required|local|Input source name.|
|INPUT_FOLDER|Optional|input-folder|Input folder name.|

### Get From Google Cloud Storage
You can change the environment variables to setup input storage. (See .env.examples)

|Key|Required|Example|Detail|
|:---|:---|:---|:---|
|INPUT_SOURCE|Required|gcs|Input source name.|
|INPUT_FOLDER|Optional|input-folder|Input folder name.|
|GOOGLE_APPLICATION_CREDENTIALS|Required|/xxxx/account_key.json|Path to GCP serivce account key.|
|GOOGLE_STORAGE_BUCKET|Required|input-bucket|GCS bucket name.|

## Change Writer Task Setting
[writer-task](https://github.com/podder-ai/writer-task) supports the following output storages.
- Local Storage
- Google Cloud Storage

### Output To Local Storage
You can change the environment variables to setup output storage. (See .env.examples)

|Key|Required|Example|Detail|
|:---|:---|:---|:---|
|OUTPUT_SOURCE|Required|local|Output source name.|
|OUTPUT_FOLDER|Optional|output-folder|Output folder name.|

### Output To Google Cloud Storage
You can change the environment variables to setup output storage. (See .env.examples)

|Key|Required|Example|Detail|
|:---|:---|:---|:---|
|OUTPUT_SOURCE|Required|gcs|Output source name.|
|OUTPUT_FOLDER|Optional|output-folder|Output folder name.|
|GOOGLE_APPLICATION_CREDENTIALS|Required|/xxxx/account_key.json|Path to GCP serivce account key.|
|GOOGLE_STORAGE_BUCKET|Required|output-bucket|GCS bucket name.|