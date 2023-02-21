# Get Player Information

The client can request the server to send the player information.

## Serverbound

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
      "const": "serverbound"
    },
    "type": {
      "const": "get_player_information_request"
    }
  }
}
```

## Clientbound

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "health",
    "experiments",
    "inventory",
    "main_hand"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "get_player_information_response"
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