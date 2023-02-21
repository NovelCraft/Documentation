# Perform Action

## Serverbound

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "unique_id"
  ],
  "properties": {
    "bound_to": {
      "const": "serverbound"
    },
    "type": {
      "const": "perform_action"
    },
    "unique_id": {
      "description": "The player's unique ID",
      "type": "integer"
    },
    "instant": {
      "description": "The instant action",
      "type": "string",
      "enum": [
        "jump",
        "attack_click",
        "attack_start",
        "attack_end",
        "use_click",
        "use_start",
        "use_end"
      ]
    },
    "inventory_drop": {
      "description": "The inventory action",
      "type": "object",
      "additionalProperties": false,
      "required": [
        "slot",
        "count"
      ],
      "properties": {
        "slot": {
          "description": "The slot index",
          "type": "integer",
          "minimum": 0,
          "maximum": 35
        },
        "count": {
          "description": "The item count",
          "type": "integer",
          "minimum": 0
        }
      }
    },
    "inventory_swap": {
      "description": "The inventory action",
      "type": "object",
      "additionalProperties": false,
      "required": [
        "slots",
      ],
      "properties": {
        "slots": {
          "description": "The slot indexes to swap",
          "type": "array",
          "minItems": 2,
          "maxItems": 2,
          "uniqueItems": true,
          "items": {
            "type": "integer",
            "minimum": 0,
            "maximum": 35
          }
        }
      }
    },
    "inventory_select": {
      "description": "The inventory action",
      "type": "object",
      "additionalProperties": false,
      "required": [
        "slot",
      ],
      "properties": {
        "slot": {
          "description": "The slot index",
          "type": "integer",
          "minimum": 0,
          "maximum": 8
        }
      }
    },
    "movement_normal": {
      "description": "The movement action",
      "type": "object",
      "additionalProperties": false,
      "required": [
        "forward",
        "sideward",
      ],
      "properties": {
        "forward": {
          "description": "Whether the player is moving forward. -1 for backward, 0 for not moving, 1 for forward",
          "enum": [-1, 0, 1]
        },
        "sideward": {
          "description": "Whether the player is moving sideward. -1 for left, 0 for not moving, 1 for right",
          "enum": [-1, 0, 1]
        }
      }
    },
    "movement_orientation": {
      "description": "The movement action",
      "type": "object",
      "additionalProperties": false,
      "required": [
        "yaw",
        "pitch",
      ],
      "properties": {
        "yaw": {
          "description": "The entity yaw",
          "mininum": 0,
          "exclusiveMaximum": 360,
          "type": "integer"
        },
        "pitch": {
          "description": "The entity pitch",
          "mininum": -90,
          "maximum": 90,
          "type": "integer"
        }
      }
    }
  }
}
```