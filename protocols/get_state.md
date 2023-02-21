# Get State

The client can request the server to send the game state.

## Serverbound

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type"
  ],
  "properties": {
    "bound_to": {
      "const": "serverbound"
    },
    "type": {
      "const": "get_state_request"
    }
  }
}
```

## Clientbound

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "state"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "get_state_response"
    },
    "state": {
      "description": "The game state",
      "enum": [
        "waiting",
        "playing",
        "paused",
        "finished"
      ]
    }
  }
}
```