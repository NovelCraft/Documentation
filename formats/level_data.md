# Level Data Format

The level data file is a zipped file containing the level data in JSON format with file name `level_data.json`.

```json
{
    "$schema": "https://json-schema.org/draft-07/schema",
    "type": "object",
    "additionalProperties": false,
    "required": [
        "type",
        "sections",
        "entities",
        "players"
    ],
    "properties": {
        "type": {
            "const": "level_data"
        },
        "sections": {
            "description": "The sections",
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
        }
    },
    "entities": {
        "description": "The entities",
        "type": "array",
        "items": {
            "description": "The entity",
            "type": "object",
            "additionalProperties": false,
            "required": [
                "type_id",
                "unique_id",
                "position",
                "orientation"
            ],
            "properties": {
                "type_id": {
                    "description": "The entity type ID",
                    "type": "integer"
                },
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
    },
    "players": {
        "description": "The players",
        "type": "array",
        "items": {
            "type": "object",
            "additionalProperties": false,
            "required": [
                "unique_id",
                "health",
                "experiments",
                "inventory",
                "main_hand"
            ],
            "properties": {
                "unique_id": {
                    "description": "The player's unique ID",
                    "type": "integer"
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
                            "item_type_id",
                            "count"
                        ],
                        "properties": {
                            "slot": {
                                "description": "The slot index",
                                "type": "integer",
                                "minimum": 0,
                                "maximum": 35
                            },
                            "item_type_id": {
                                "description": "The item type id",
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
    }
}
```