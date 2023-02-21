# Update Blocks

When blocks around the player are updated, the server will send a packet to the client to update the blocks. The section position is the position of the section whose coordinates are the minimum of the coordinates of the blocks in the section.

The position of the block in the section is the position of the block relative to the section.

## Clientbound

In every tick, the server will send a packet to the client to update the block changes around the player.

When the player moves to a new section, the server will send a packet to the client to update the blocks in the sections around the player which cannot be seen by the player before.

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "blocks"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "update_block_changes"
    },
    "blocks": {
      "description": "The blocks in the section",
      "type": "array",
      "items": {
        "type": "object",
        "additionalProperties": false,
        "required": [
          "x",
          "y",
          "z",
          "block_id"
        ],
        "properties": {
          "x": {
            "description": "The x coordinate of the block",
            "type": "integer"
          },
          "y": {
            "description": "The y coordinate of the block",
            "type": "integer"
          },
          "z": {
            "description": "The z coordinate of the block",
            "type": "integer"
          },
          "block_id": {
            "description": "The block ID",
            "type": "integer"
          }
        }
      }
    }
  }
}
```