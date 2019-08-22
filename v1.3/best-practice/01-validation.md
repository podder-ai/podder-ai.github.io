# Validation
In order to keep your datas safe without errors, it is recommended for you to make validations for your project.

## JSON Schema
JSON Schema allows users to define requirements for JSON, and will be able to test whether JSON is in expected format easily.

### JSON Schema in Podder
In Podder, JSON schema is recommended to validate result output for each necessary tasks.

For executing validation using JSON schema, please create JSON schema  file in `config` directory, and run validation using JSON schema library.

Here are the sample code of running JSON schema validation within a task.

```
import json
from jsonschema import validate

class SampleTask(object):
    // Define file name of JSON schema file.
    JSON_SCHEMA_FILE = '...'

    def __init__(self, context: Context) -> None:
        self.context = context

    def execute(self, input_data: Any) -> Any:
        // Code logic to get your output result,for validating using JSON schema.
        params = ...

        // Run JSON schema
        schema_file_path = self.context.file.get_config_path(self.JSON_SCHEMA_FILE)
        with open(schema_file_path, 'r') as schema_file:
            schema = json.load(schema_file)
            validate(instance=params, schema=schema)
```


### About JSON Schema
JSON schema is written in JSON format, and will have following parameters.

#### $schema
Determines which version of JSON schema to use. Current newest version at June 2019, is `draft-04`.

#### type
Determines types of key values. Please specify `object` for this.

#### properties
Determines names of each objects in JSON. Please specify key names that will be included in result of post process tasks.

Keys specified as `properties` does not have to be included in every JSON output. For keys that must be included in every JSON output, please use `required` parameter.

#### required
Determines properties that must be included in JSON. If there is any keys missing from keys specified in `required` parameter, JSON schema will return error output.


#### Sample of JSON Schema
Following JSON is a sample of simple JSON schema.

This JSON uses newest schema of `draft-04`, and type of each keys set as `object`. This JSON has with 5 keys specified as `properties`, and 2 keys specified as `required`.

In following case, JSON schema will output error if `job_id` or `status_code` does not exist in JSON output. But even if `start_time`, `end_time` and `during_time` does not exist in JSON output, JSON schema will not output error, and collects data only if keys exists in JSON output.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "properties": {
    "job_id": {
      "type": "string"
    },
    "status_code": {
      "type": "integer"
    },
    "start_time": {
      "type": "string"
    },
    "end_time": {
      "type": "string"
    },
    "during_time": {
      "type": "string"
    },
  },
  "required": [
    "job_id",
    "status_code"
  ]
}
```
