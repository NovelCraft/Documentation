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
        "type_id"
      ],
      "properties": {
        "identifier": {
          "type": "string"
        },
        "type_id": {
          "type": "integer"
        },
      }
    },
    "components": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "attack": {
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
        },
        "collision_box": {
          "type": "object",
          "additionalProperties": false,
          "required": [
            "height",
            "width",
            "eye_height"
          ],
          "properties": {
            "height": {
              "type": "number",
              "exclusiveMinimum": 0
            },
            "width": {
              "type": "number",
              "exclusiveMinimum": 0
            },
            "eye_height": {
              "type": "number",
              "exclusiveMinimum": 0
            }
          }
        },
        "health": {
          "type": "object",
          "additionalProperties": false,
          "required": [
            "value",
            "max"
          ],
          "properties": {
            "value": {
              "type": "number",
              "exclusiveMinimum": 0
            },
            "max": {
              "type": "number",
              "exclusiveMinimum": 0
            }
          }
        },
        "movement": {
          "type": "object",
          "additionalProperties": false,
          "required": [
            "value"
          ],
          "properties": {
            "value": {
              "type": "number",
              "minimum": 0
            }
          }
        },
        "movement.jump": {
          "type": "object",
          "additionalProperties": false,
          "required": [
            "value"
          ],
          "properties": {
            "value": {
              "type": "number",
              "minimum": 0
            }
          }
        },
        "physics": {
          "type": "object",
          "additionalProperties": false
        }
      }
    }
  }
}
```