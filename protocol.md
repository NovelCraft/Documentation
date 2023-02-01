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
    "unique_id"
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
    "unique_id": {
      "description": "The unique id of the player",
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

## Get Player Information

The client can request the server to send the player information.

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
      "const": "get_player_information_request"
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
          "unique_id"
        ],
        "properties": {
          "entity_id": {
            "description": "The entity ID",
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

## Update Player

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

## Update Ticks

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
    "ticks"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "update_ticks"
    },
    "ticks": {
      "description": "The ticks since the game started",
      "type": "integer"
    }
  }
}
```

## Perform Action

### Serverbound

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