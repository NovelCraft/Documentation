# Update Player

## Clientbound

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "update_player"
    },
    "health": {
      "description": "The player health",
      "type": "number",
      "minimum": 0,
      "maximum": 20
    },
    "experiments": {
      "description": "The player experiments",
      "type": "integer",
      "minimum": 0
    },
    "inventory": {
      "description": "The player inventory. The first 9 slots are hotbar slots.",
      "type": "array",
      "maxItems": 36,
      "items": {
        "type": "object",
        "additionalProperties": false,
        "required": [
          "slot",
          "item_id",
          "count",
          "damage"
        ],
        "properties": {
          "slot": {
            "description": "The slot index",
            "type": "integer",
            "minimum": 0,
            "maximum": 35
          },
          "item_id": {
            "description": "The item id",
            "type": "integer"
          },
          "count": {
            "description": "The item count",
            "type": "integer",
            "minimum": 0
          }
        }
      }
    },
    "main_hand": {
      "description": "The main hand selected slot index",
      "type": "integer",
      "minimum": 0,
      "maximum": 8
    }
  }
}
```