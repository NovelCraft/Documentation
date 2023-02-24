# Record Format

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "type",
    "records"
  ],
  "properties": {
    "type": {
      "const": "record"
    },
    "records": {
      "type": "array",
      "items": {
        "type": "object",
        "required": [
          "type",
          "identifier",
          "ticks",
          "data"
        ],
        "properties": {
          "type": {
            "type": "enum",
            "values": [
              "event"
            ]
          },
          "identifier": {
            "type": "string",
            "description": "The identifier of the event, or the file name of the event's documentation."
          },
          "ticks": {
            "type": "integer",
            "minimum": 0
          },
          "data": {
            "type": "object",
            "description": "The data for the event. Described in the event's documentation."
          }
        }
      }
    }
  }
}
```