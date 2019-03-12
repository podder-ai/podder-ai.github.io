# Advanced Settings
In this section, you will get the following things:
- Change Sensor Task Setting
- Change Writer Task Setting


## Change Sensor Task Setting

## Change Writer Task Setting
[Writer Task](https://github.com/podder-ai/writer-task) supports the following storages to output your results.
- Local Storage (Default)
- Google Cloud Storage

### Output To Local Storage
You can change the environment variables to change output storage. (See .env.examples)

|Key|Required|Example|Detail|
|:---|:---|:---|:---|
|OUTPUT_SOURCE|Required|local|Output source name.|
|OUTPUT_FOLDER|Optional|sample-folder|Output folder name.|

### Output To Google Cloud Storage
You can change the environment variables to change output storage. (See .env.examples)

|Key|Required|Example|Detail|
|:---|:---|:---|:---|
|OUTPUT_SOURCE|Required|gcs|Output source name.|
|OUTPUT_FOLDER|Optional|sample-folder|Output folder name.|
|GOOGLE_APPLICATION_CREDENTIALS|Required|/xxxx/account_key.json|Path to GCP serivce account key.|
|GOOGLE_STORAGE_BUCKET|Required|sample-folder|GCS bucket name.|