# Item Definition Format

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
      "const": "item"
    },
    "description": {
      "type": "object",
      "additionalProperties": false,
      "required": [
        "identifier",
        "item_id"
      ],
      "properties": {
        "identifier": {
          "type": "string"
        },
        "item_id": {
          "type": "integer",
          "minimum": 0
        },
      }
    },
    "components": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "max_stack_size": {
          "type": "integer",
          "minimum": 1,
          "maximum": 64
        },
      }
    }
  }
}
```