# Communication Protocols

## Packet Format

NovelCraft transmits data via WebSocket protocol. Each data packet is a JSON object with the format specified by the following JSON schema:

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "required": [
    "bound_to",
    "type"
  ],
  "properties": {
    "bound_to": {
      "description": "The direction of the packet",
      "enum": [
        "serverbound",
        "clientbound"
      ]
    },
    "type": {
      "description": "The type of the packet",
      "type": "string"
    }
  }
}
```

When an error occurs, the server will send a packet with the following format:

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "code",
    "message"
  ],
  "properties": {
    "bound_to": {
      "description": "The direction of the packet",
      "enum": [
        "serverbound",
        "clientbound"
      ]
    },
    "type": {
      "const": "error"
    },
    "code": {
      "description": "The error code",
      "type": "integer"
    },
    "message": {
      "description": "The error message",
      "type": "string"
    }
  }
}
```

`serverbound` packets are sent from the client to the server, and `clientbound` packets are sent from the server to the client.