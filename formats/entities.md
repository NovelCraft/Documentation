# Entity Definition Format

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
      "const": "entity"
    },
    "description": {
      "type": "object",
      "additionalProperties": false,
      "required": [
        "identifier",
        "entity_id"
      ],
      "properties": {
        "identifier": {
          "type": "string"
        },
        "entity_id": {
          "type": "integer",
          "minimum": 0
        },
      }
    },
    "components": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "collision_box": {
          "type": "object",
          "additionalProperties": false,
          "required": [
            "width",
            "height"
          ],
          "properties": {
            "width": {
              "type": "number",
              "exclusiveMinimum": 0
            },
            "height": {
              "type": "number",
              "exclusiveMinimum": 0
            }
          }
        }
      }
    }
  }
}
```