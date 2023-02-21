# Update Ticks

In every second, the server will send a packet to update the time.

## Clientbound

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "ticks"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "update_ticks"
    },
    "ticks": {
      "description": "The ticks since the game started",
      "type": "integer"
    }
  }
}
```