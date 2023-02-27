# AfterEntitySpawn Event Record

`type` is `event` and `data` is an object with the following properties:

```json
{
    "type": "object",
    "required": [
        "spawn_list",
    ],
    "properties": {
        "spawn_list":{
            "type": "array",
            "items": {
                "type": "object",
                "required": [
                    "unique_id",
                ],
                "properties": {
                    "unique_id": {
                        "type": "integer"
                    }
                }
            }
        }
    }
}
```