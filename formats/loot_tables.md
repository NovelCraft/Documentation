# Loot Table Definition Format

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "type",
    "description",
    "pools"
  ],
  "properties": {
    "type": {
      "const": "loot_table"
    },
    "description": {
      "type": "object",
      "additionalProperties": false,
      "required": [
        "type",
        "loot_table_id"
      ],
      "properties": {
        "type": {
          "enum": [
            "block",
            "entity"
          ]
        },
        "loot_table_id": {
          "type": "integer"
        },
      }
    },
    "pools": {
      "type": "array",
      "items": {
        "type": "object",
        "additionalProperties": false,
        "required": [
          "rolls",
          "entries"
        ],
        "properties": {
          "rolls": {
            "type": "integer",
            "minimum": 0
          },
          "entries": {
            "type": "object",
            "additionalProperties": false,
            "required": [
              "weight"
            ],
            "properties": {
              "weight": {
                "type": "number",
                "minimum": 0
              },
              "item_id": {
                "description": "The item id of the item to drop. If not specified, it will be treated as nothing.",
                "type": "integer",
                "minimum": 0
              },
            }
          }
        }
      }
    }
  }
}
```