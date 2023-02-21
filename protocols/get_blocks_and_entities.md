# Get Blocks and Entities

The client can request the server to send the blocks and entities around the player.

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
      "const": "get_blocks_and_entities_request"
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
    "sections",
    "entities"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "get_blocks_and_entities_response"
    },
    "sections": {
      "description": "The sections to update",
      "type": "array",
      "items": {
        "type": "object",
        "additionalProperties": false,
        "required": [
          "x",
          "y",
          "z",
          "blocks"
        ],
        "properties": {
          "x": {
            "description": "The x coordinate of the section",
            "type": "integer"
          },
          "y": {
            "description": "The y coordinate of the section",
            "type": "integer"
          },
          "z": {
            "description": "The z coordinate of the section",
            "type": "integer"
          },
          "blocks": {
            "description": "The blocks in the section which can be accessed by `blocks[x*256+y*16+z]`",
            "type": "array",
            "minItems": 4096,
            "maxItems": 4096,
            "items": {
              "description": "The block ID",
              "type": "integer"
            }
          }
        }
      }
    },
    "entities": {
      "description": "The entities",
      "type": "array",
      "items": {
        "description": "An entity",
        "type": "object",
        "additionalProperties": false,
        "required": [
          "unique_id",
          "position",
          "orientation"
        ],
        "properties": {
          "unique_id": {
            "description": "The entity unique ID",
            "type": "integer"
          },
          "position": {
            "description": "The entity position",
            "type": "object",
            "additionalProperties": false,
            "required": [
              "x",
              "y",
              "z"
            ],
            "properties": {
              "x": {
                "description": "The x coordinate of the entity",
                "type": "number"
              },
              "y": {
                "description": "The y coordinate of the entity",
                "type": "number"
              },
              "z": {
                "description": "The z coordinate of the entity",
                "type": "number"
              }
            }
          },
          "orientation": {
            "description": "The entity orientation",
            "type": "object",
            "additionalProperties": false,
            "required": [
              "yaw",
              "pitch"
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
    }
  }
}
```