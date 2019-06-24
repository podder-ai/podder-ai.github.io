# Best Practice
In order to keep your datas safe and validated, it is recommended for you to create validators for your project.

## JSON Schema
JSON Schema allows users to define requirements for JSON, and will be able to test whether JSON is in expected format easily.

In Podder, JSON schema is used to validate result for post process tasks. And to update JSON schema, please update `success_json_schema.json` file, in `config` directory.

JSON schema is written in JSON format, and will have following parameters.

### JSON Schema parameters
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
