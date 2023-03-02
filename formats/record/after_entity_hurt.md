# AfterEntityHurt Event Record

`type` is `event` and `data` is an object with the following properties:

```json
{
    "type": "object",
    "required": [
        "hurt_list"
    ],
    "properties": {
        "hurt_list": {
            "type": "array",
            "items": {
                "type": "object",
                "required": [
                    "victim_unique_id",
                    "damage"
                ],
                "properties": {
                    "victim_unique_id": {
                        "type": "integer"
                    },
                    "damage": {
                        "type": "number"
                    }
                }
            }
        }
    }
}
```