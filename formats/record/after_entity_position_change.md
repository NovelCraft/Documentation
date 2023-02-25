# AfterEntityPositionChange Event Record

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
                    "unique_id",
                    "position"
                ],
                "properties": {
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
                    }
                }
            }
        }
    }
}
```