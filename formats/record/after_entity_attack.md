# AfterEntityAttack Event Record

`type` is `event` and `data` is an object with the following properties:

```json
{
    "type": "object",
    "required": [
        "attack_list"
    ],
    "properties": {
        "attack_list": {
            "type": "array",
            "items": {
                "type": "object",
                "required": [
                    "attacker_unique_id",
                    "attack_kind"
                ],
                "properties": {
                    "attacker_unique_id": {
                        "type": "integer"
                    },
                    "attack_kind": {
                        "enum": [
                            "click",
                            "hold_start",
                            "hold_end"
                        ]
                    }
                }
            }
        }
    }
}
```