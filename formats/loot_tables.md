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
        "block_or_entity_type_id"
      ],
      "properties": {
        "type": {
          "enum": [
            "block",
            "entity"
          ]
        },
        "block_or_entity_type_id": {
          "type": "integer"
        }
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
              "item_type_id": {
                "description": "The type id of the item to drop. If not specified, it will be treated as nothing.",
                "type": "integer",
                "minimum": 0
              },
            }
          }
        }
      }
    },
    "only_loot_by": {
      "type": "array",
      "items": {
        "type": "integer"
      }
    }
  }
}
```