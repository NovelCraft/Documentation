# Block Definition Format

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "type",
    "description",
    "components"
  ],
  "properties": {
    "type": {
      "const": "block"
    },
    "description": {
      "type": "object",
      "additionalProperties": false,
      "required": [
        "identifier",
        "block_id"
      ],
      "properties": {
        "identifier": {
          "type": "string"
        },
        "block_id": {
          "type": "integer",
          "minimum": 0
        },
      }
    },
    "components": {
      "type": "object",
      "additionalProperties": false,
      "properties": {}
    }
  }
}
```