# AfterPlayerInventoryChange Event Record

`type` is `event` and `data` is an object with the following properties:

```json
{
    "type": "object",
    "required": [
        "change_list",
    ],
    "properties": {
        "change_list":{
            "type": "array",
            "items": {
                "type": "object",
                "required": [
                    "player_unique_id",
                    "change_list"
                ],
                "properties": {
                    "unique_id": {
                        "type": "integer"
                    },
                    "change_list": {
                        "type": "array",
                        "items": {
                            "type": "object",
                            "required": [
                                "slot",
                                "count"
                            ],
                            "properties": {
                                "slot": {
                                    "type": "integer"
                                },
                                "count": {
                                    "type": "integer"
                                },
                                "item_type_id": {
                                    "type": "string"
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