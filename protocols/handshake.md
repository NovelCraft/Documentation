# Handshake

When a player connects to the server, the client will send a handshake packet to the server. The server will respond with a handshake packet. If the client already has a token, it will be sent to the server. If the client does not have a token, the server will send a `null` token to the client, and the client will send a handshake packet to the server again with the token obtained from the server.

## Serverbound

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "bound_to",
    "type",
    "protocol_version",
    "token"
  ],
  "properties": {
    "bound_to": {
      "const": "serverbound"
    },
    "type": {
      "description": "The type of the packet",
      "const": "handshake"
    },
    "protocol_version": {
      "description": "The protocol version for communication",
      "enum": [
        1
      ]
    },
    "token": {
      {
        "description": "The token of the player. Empty to request a token",
        "type": "string"
      }
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
    "token",
    "unique_id"
  ],
  "properties": {
    "bound_to": {
      "const": "clientbound"
    },
    "type": {
      "const": "handshake"
    },
    "token": {
      "description": "The token of the player",
      "type": "string"
    },
    "unique_id": {
      "description": "The unique id of the player",
      "type": "integer"
    }
  }
}
```