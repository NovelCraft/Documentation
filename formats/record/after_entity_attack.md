# AfterEntityAttack Event Record

`type` is `event` and `data` is an object with the following properties:

```json
{
    "type": "object",
    "required": [
        "attacker_unique_id",
        "attack_type"
    ],
    "properties": {
        "attacker_unique_id": {
            "type": "integer"
        },
        "attack_type": {
            "enum": [
                "click",
                "hold_start",
                "hold_end"
            ]
        }
    }
}
```