# Update Entities

When entities around the player are updated, the server will send a packet to the client to update the entities.

## Clientbound

In every tick, the server will send a packet to update the changed entities.

When the player moves to a new section, the server will send a packet to the client to update the entities in the sections around the player which cannot be seen by the player before.

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "entities"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "update_entity_changes"
    },
    "entities": {
      "description": "The entities to update",
      "type": "array",
      "items": {
        "description": "The entity to update",
        "type": "object",
        "additionalProperties": false,
        "required": [
          "type_id",
          "unique_id"
        ],
        "properties": {
          "type_id": {
            "description": "The entity type ID",
            "type": "integer"
          },
          "unique_id": {
            "description": "The entity's unique ID",
            "type": "integer"
          },
          "data_value": {
            "description": "The entity data value",
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
          },
          "event": {
            "description": "The entity event",
            "enum": [
              "spawn",
              "despawn"
            ]
          }
        }
      }
    }
  }
}
```