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
  "$schema": "https://json-schema.org/draft/2020-12/schema",
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
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "additionalProperties": false,
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
      "const": "error"
    }
  }
}
```

## Handshake

When a player connects to the server, the client will send a handshake packet to the server. The server will respond with a handshake packet. If the client already has a token, it will be sent to the server. If the client does not have a token, the server will send a `null` token to the client, and the client will send a handshake packet to the server again with the token obtained from the server.

### Serverbound

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
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
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "token"
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
    }
  }
}
```

## Update Blocks

When blocks around the player are updated, the server will send a packet to the client to update the blocks. The section position is the position of the section whose coordinates are the minimum of the coordinates of the blocks in the section.

The position of the block in the section is the position of the block relative to the section.

### Clientbound

In every tick, the server will send a packet to the client to update the block changes around the player.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
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
                  "type": "number"
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

In every 20 ticks, the server will send a packet to the client to update all the blocks around the player.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
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
      "const": "update_blocks"
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
                  "type": "number"
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

## Update Entities

When entities around the player are updated, the server will send a packet to the client to update the entities.

### Clientbound

In every tick, the server will send a packet to update the changed entities.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
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
          "yaw",
          "pitch",
          "entity_definiion"
        ],
        "properties": {
          "entity_id": {
            "description": "The entity unique ID",
            "type": "number"
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
          "yaw": {
            "description": "The entity yaw",
            "mininum": 0,
            "exclusiveMaximum": 360,
            "type": "number"
          },
          "pitch": {
            "description": "The entity pitch",
            "mininum": -90,
            "maximum": 90,
            "type": "number"
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

In every 20 ticks, the server will send a packet to update all the entities around the player.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
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
      "const": "update_entities"
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
          "yaw",
          "pitch",
          "entity_definiion"
        ],
        "properties": {
          "entity_id": {
            "description": "The entity unique ID",
            "type": "number"
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
          "yaw": {
            "description": "The entity yaw",
            "mininum": 0,
            "exclusiveMaximum": 360,
            "type": "number"
          },
          "pitch": {
            "description": "The entity pitch",
            "mininum": -90,
            "maximum": 90,
            "type": "number"
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

## Update Time

In every second, the server will send a packet to update the time.

### Clientbound

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
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
      "type": "number"
    },
    "ticks": {
      "description": "The ticks since the server started",
      "type": "number"
    }
  }
}
```

```markdown
## 玩家自己的包 server -> client

每一tick都要发送

| 数据名称   | 数据类型     | 描述       |
|:------:|:--------:|:--------:|
| 玩家ID   | byte     |          |
| 玩家血量   | decimal  | 0.0-10.0 |
| 玩家经验值  | int      |          |
| 玩家总分  | int      |   历史经验值的总和       |
| 玩家朝向   | Position | 正前方的单位向量 |
| 玩家坐标   | Position |          |
| 玩家速度   | Position | 速度向量     |
| 玩家背包   | 36*Item  |          |
| 玩家手持物品 | Item     |          |

## 玩家自己的按键包 client -> server

需要该操作才发送

| 数据名称     | 数据类型 | 描述  |
|:--------:|:----:|:---:|
| 玩家ID     | byte |     |
| 玩家是否按下左键 | Bool |     |
| 玩家是否按下右键 | Bool |     |
| 玩家是否跳跃 | Bool |     |

## 玩家自己的物品包 client -> server

需要该操作才发送

| 数据名称           | 数据类型              | 描述                                  |
|:--------------:|:-----------------:|:-----------------------------------:|
| 玩家ID           | byte              |                                     |
| 玩家丢弃的物品在背包中的序号 | int               | 负数为不丢弃，对于[0,35]的整数，背包对应位置存在非空物品才能丢弃 |
| 玩家背包交换物品       | pair<int x,int y> | 负数为不丢弃，       交换x与y物品               |
```