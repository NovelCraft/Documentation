# Logging Definition Format

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "type",
    "logging"
  ],
  "properties": {
    "type": {
      "const": "logging"
    },
    "logging": {
      "type": "array",
      "items": {
        "description": "A logging item",
        "type": "object",
        "required": [
          "ticks"
        ],
        "properties": {
          "ticks": {
            "type": "integer"
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
                "orientation"
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
                }
              }
            }
          },
          "players": {
            "description": "The players to update",
            "type": "array",
            "items": {
              "type": "object",
              "additionalProperties": false,
              "required": [
                "entity_id",
                "token",
                "health",
                "experiments",
                "inventory",
                "main_hand"
              ],
              "properties": {
                "entity_id": {
                  "description": "The entity unique ID",
                  "type": "integer"
                },
                "token": {
                  "description": "The player token",
                  "type": "string"
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
          },
          "actions": {
            "description": "The actions performed.",
            "type": "array",
            "itmes": {
              "type": "object",
              "additionalProperties": false,
              "required": [
                "entity_id"
              ],
              "properties": {
                "entity_id": {
                  "description": "The entity unique ID",
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
                },
              }
            }
          }
        }
      }
    }
  }
}
```