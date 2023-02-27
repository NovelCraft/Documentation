# AfterBlockChange Event Record

`type` is `event` and `data` is an object with the following properties:

```json
{
    "type": "object",
    "required": [
        "change_list"
    ],
    "properties": {
        "change_list": {
            "type": "array",
            "items": {
                "type": "object",
                "required": [
                    "position",
                    "block_type_id"
                ],
                "properties": {
                    "position": {
                        "type": "object",
                        "required": [
                            "x",
                            "y",
                            "z"
                        ],
                        "properties": {
                            "x": {
                                "type": "integer"
                            },
                            "y": {
                                "type": "integer"
                            },
                            "z": {
                                "type": "integer"
                            }
                        }
                    },
                    "block_type_id": {
                        "type": "integer"
                    }
                }
            }
        }
    }
}
```