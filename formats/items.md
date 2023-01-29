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
          "type": "integer"
        },
      }
    },
    "components": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "digger": {
          "type": "object",
          "additionalProperties": false,
          "required": [
            "destroy_speeds"
          ],
          "properties": {
            "destroy_speeds": {
              "type": "array",
              "minItems": 1,
              "items": {
                "type": "object",
                "additionalProperties": false,
                "required": [
                  "block_id",
                  "speed_modifier"
                ],
                "properties": {
                  "block_id": {
                    "type": "integer"
                  },
                  "speed_modifier": {
                    "type": "number",
                    "minimum": 0
                  }
                }
              }
            }
          }
        },
        "food": {
          "type": "object",
          "additionalProperties": false,
          "required": [
            "can_always_eat",
            "nutrition"
          ],
          "properties": {
            "can_always_eat": {
              "type": "boolean"
            },
            "nutrition": {
              "type": "number",
              "minimum": 0
            }
          }
        },
        "max_stack_size": {
          "type": "integer",
          "minimum": 1,
          "maximum": 64
        },
        "weapon": {
          "type": "object",
          "additionalProperties": false,
          "required": [
            "damage"
          ],
          "properties": {
            "damage": {
              "type": "number",
              "minimum": 0
            }
          }
        }
      }
    }
  }
}
```