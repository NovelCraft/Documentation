# AfterEntityOrientationChange Event Record

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
                    "orientation"
                ],
                "properties": {
                    "unique_id": {
                        "type": "integer"
                    },
                    "orientation": {
                        "type": "object",
                        "required": [
                            "yaw",
                            "pitch",
                        ],
                        "properties": {
                            "yaw": {
                                "type": "number"
                            },
                            "pitch": {
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