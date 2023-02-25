# AfterEntityCreate Event Record

`type` is `event` and `data` is an object with the following properties:

```json
{
    "type": "object",
    "required": [
        "entity_type_id",
        "creation_list",
        "unique_id",
        "position"
    ],
    "properties": {
        {
            "type": "array",
            "items": {
                "type": "object",
                "required": [
                    "entity_type_id",
                    "unique_id",
                    "position",
                    "orientation"
                ],
                "properties": {
                    "entity_type_id": {
                        "type": "integer"
                    },
                    "unique_id": {
                        "type": "integer"
                    },
                    "position": {
                        "type": "object",
                        "required": [
                            "x",
                            "y",
                            "z"
                        ],
                        "properties": {
                            "x": {
                                "type": "number"
                            },
                            "y": {
                                "type": "number"
                            },
                            "z": {
                                "type": "number"
                            }
                        }
                    },
                    "orientation": {
                        "type": "object",
                        "required": [
                            "yaw",
                            "pitch"
                        ],
                        "properties": {
                            "yaw": {
                                "type": "number"
                            },
                            "pitch": {
                                "type": "number"
                            }
                        }
                    },
                    "item_type_id": {
                        "type": "number",
                        "description": "The item type id of the entity. Only present if entity_type_id is 1 (item_entity)."
                    }
                }
            }
        }
    }
}
```