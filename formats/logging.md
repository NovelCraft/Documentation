# Logging Definition Format

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "type",
    "logging"
  ],
  "properties": {
    "type": {
      "const": "logging"
    },
    "logging": {
      "type": "array",
      "items": {
        "description": "A logging item",
        "type": "object"
      }
    }
  }
}
```