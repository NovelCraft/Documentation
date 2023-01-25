# Protocol

This page presents the protocol of NovelCraft.

## Definitions

### Data Types

All data sent over the network is encoded in JSON format. Therefore, we adopt these JSON data types:

- Number: a signed decimal number that may contain a fractional part and may use exponential E notation, but cannot include non-numbers such as NaN. The format makes no distinction between integer and floating-point.

- String: a sequence of zero or more Unicode characters.

- Boolean: either of the values `true` or `false`

- Array: an ordered list of zero or more elements, each of which may be of any type.

- Object: a collection of name–value pairs where the names (also called keys) are strings.

- `null`: an empty value, using the word `null`

## Packet Format

NovelCraft transmits data via WebSocket protocol. Each data packet is a JSON object with the format specified by the following JSON schema:

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "required": [
    "bound_to",
    "type"
  ],
  "properties": {
    "bound_to": {
      "description": "The direction of the packet",
      "enum": [
        "serverbound",
        "clientbound"
      ]
    },
    "type": {
      "description": "The type of the packet",
      "type": "string"
    }
  }
}
```

The data packets listed below only list the data part of the packet.

When an error occurs, the server will send a packet with the following format:

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "code",
    "message"
  ],
  "properties": {
    "bound_to": {
      "description": "The direction of the packet",
      "enum": [
        "serverbound",
        "clientbound"
      ]
    },
    "type": {
      "const": "error"
    },
    "code": {
      "description": "The error code",
      "type": "integer"
    },
    "message": {
      "description": "The error message",
      "type": "string"
    }
  }
}
```

## Handshake

When a player connects to the server, the client will send a handshake packet to the server. The server will respond with a handshake packet. If the client already has a token, it will be sent to the server. If the client does not have a token, the server will send a `null` token to the client, and the client will send a handshake packet to the server again with the token obtained from the server.

### Serverbound

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "protocol_version",
    "token"
  ],
  "properties": {
    "bound_to": {
      "const": "serverbound"
    },
    "type": {
      "description": "The type of the packet",
      "const": "handshake"
    },
    "protocol_version": {
      "description": "The protocol version for communication",
      "enum": [
        1
      ]
    },
    "token": {
      {
        "description": "The token of the player. Empty to request a token",
        "type": "string"
      }
    }
  }
}
```

### Clientbound

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "token",
    "entity_id"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "handshake"
    },
    "token": {
      "description": "The token of the player",
      "type": "string"
    },
    "entity_id": {
      "description": "The entity id of the player",
      "type": "integer"
    }
  }
}
```

## Get Blocks and Entities

The client can request the server to send the blocks and entities around the player.

### Serverbound

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

### Clientbound

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
            "description": "The blocks in the section which can be accessed by `blocks[x][y][z]`",
            "type": "array",
            "minItems": 16,
            "maxItems": 16,
            "items": {
              "type": "array",
              "minItems": 16,
              "maxItems": 16,
              "items": {
                "type": "array",
                "minItems": 16,
                "maxItems": 16,
                "items": {
                  "description": "The block ID",
                  "type": "integer"
                }
              }
            }
          }
        }
      }
    }
  }
}
```

## Get State

The client can request the server to send the game state.

### Serverbound

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
      "const": "get_state_request"
    }
  }
}
```

### Clientbound

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "state"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "get_state_response"
    },
    "state": {
      "description": "The game state",
      "enum": [
        "waiting",
        "playing",
        "paused",
        "finished"
      ]
    }
  }
}
```

## Update Blocks

When blocks around the player are updated, the server will send a packet to the client to update the blocks. The section position is the position of the section whose coordinates are the minimum of the coordinates of the blocks in the section.

The position of the block in the section is the position of the block relative to the section.

### Clientbound

In every tick, the server will send a packet to the client to update the block changes around the player.

When the player moves to a new section, the server will send a packet to the client to update the blocks in the sections around the player which cannot be seen by the player before.

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
      "const": "update_block_changes"
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
    },
    "entities": {
      "description": "The entities to update",
      "type": "array",
      "items": {
        "description": "The entity to update",
        "type": "object",
        "additionalProperties": false,
        "required": [
          "entity_id",
          "position",
          "orientation",
          "entity_definiion"
        ],
        "properties": {
          "entity_id": {
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
          },
          "entity_definition" {
            "description": "The entity definition",
            "type": "object"
          }
        }
      }
    }
  }
}
```

## Update Entities

When entities around the player are updated, the server will send a packet to the client to update the entities.

### Clientbound

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
          "entity_id",
          "position",
          "orientation",
          "entity_definiion"
        ],
        "properties": {
          "entity_id": {
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
          },
          "entity_definition" {
            "description": "The entity definition",
            "type": "object"
          }
        }
      }
    }
  }
}
```

## Update Player Information

### Clientbound

When a player's information is updated, the server will send a packet to the client.

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "health",
    "experiments"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "player_update_status"
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
    }
  }
}
```

## Update Player Inventory

### Clientbound

When player inventory is updated, the server will send a packet to the client. All slots should be sent except empty slots.

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "inventory",
    "main_hand"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "player_update_inventory"
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
          "id",
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
          "id": {
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

## Update Time

In every second, the server will send a packet to update the time.

### Clientbound

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "time",
    "ticks"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "update_time"
    },
    "time": {
      "description": "The time in milliseconds since the server started",
      "type": "integer"
    },
    "ticks": {
      "description": "The ticks since the server started",
      "type": "integer"
    }
  }
}
```

## Perform Instant Action

When player do some actions, such as jumping, attacking, using items, the client will send a packet to the server.

### Serverbound

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "action"
  ],
  "properties": {
    "bound_to": {
      "const": "serverbound"
    },
    "type": {
      "const": "perform_instant_action"
    },
    "action": {
      "description": "The action the player did",
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
    }
  }
}
```

## Perform Inventory Action

When player perform an inventory action, such as moving items, dropping items, the client will send a packet to the server.

### Serverbound

Players can operate the inventory by sending a packet to the server.

For dropping items:

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "slot",
    "count"
  ],
  "properties": {
    "bound_to": {
      "const": "serverbound"
    },
    "type": {
      "const": "perform_inventory_action_drop"
    },
    "slot": {
      "description": "The slot index",
      "type": "integer",
      "minimum": 0,
      "maximum": 35
    },
    "count": {
      "description": "The item count to drop",
      "type": "integer",
      "minimum": 0
    }
  }
}
```

For swapping items in inventory:

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "slots"
  ],
  "properties": {
    "bound_to": {
      "const": "serverbound"
    },
    "type": {
      "const": "perform_inventory_action_swap"
    },
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
}
```

For selecting main hand:

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "slot"
  ],
  "properties": {
    "bound_to": {
      "const": "serverbound"
    },
    "type": {
      "const": "perform_inventory_action_select"
    },
    "slot": {
      "description": "The slot index",
      "type": "integer",
      "minimum": 0,
      "maximum": 8
    }
  }
}
```

## Perform Movement

When player move, the client will send a packet to the server.

### Serverbound

For normal movement:

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "forward",
    "sideward",
  ],
  "properties": {
    "bound_to": {
      "const": "serverbound"
    },
    "type": {
      "const": "perform_movement_normal"
    },
    "forward": {
      "description": "Whether the player is moving forward. -1 for backward, 0 for not moving, 1 for forward",
      "enum": [-1, 0, 1]
    },
    "sideward": {
      "description": "Whether the player is moving sideward. -1 for left, 0 for not moving, 1 for right",
      "enum": [-1, 0, 1]
    }
  }
}
```

For updating orientation:

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "orientation"
  ],
  "properties": {
    "bound_to": {
      "const": "serverbound"
    },
    "type": {
      "const": "perform_movement_orientation"
    },
    "orientation": {
      "description": "The player orientation",
      "type": "object",
      "additionalProperties": false,
      "required": [
        "yaw",
        "pitch"
      ],
      "properties": {
        "yaw": {
          "description": "The player yaw",
          "mininum": 0,
          "exclusiveMaximum": 360,
          "type": "integer"
        },
        "pitch": {
          "description": "The player pitch",
          "mininum": -90,
          "maximum": 90,
          "type": "integer"
        }
      }
    }
  }
}
```