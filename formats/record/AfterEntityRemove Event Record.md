AfterEntityRemove Event Record

`type` is `event` and `data` is an object with the following properties:

```json
{
    "type": "object",
    "required": [
        "removal_list",
    ],
    "properties": {
        "removal_list":{
            "type": "array",
            "items": {
                "type": "object",
                "required": [
                    "unique_id"
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
