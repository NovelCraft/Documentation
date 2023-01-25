# Recipe Definition Format

```json
{
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "type",
    "recipes"
  ],
  "properties": {
    "type": {
      "const": "recipe"
    },
    "recipes": {
      "oneOf": [
        {
          "type": "object",
          "additionalProperties": false,
          "required": [
            "type",
            "is_shapeless",
            "ingredients",
            "result"
          ],
          "properties": {
            "type": {
              "const": "crafting"
            },
            "is_shapeless": {
              "type": "boolean"
            },
            "ingredients": {
              "description": "2D array of item IDs. If all items are in the top-left four slots, the crafting can be performed without crafting table.",
              "type": "array",
              "items": {
                "type": "array",
                "items": {
                  "oneOf": [
                    {
                      "description": "Item ID",
                      "type": "integer",
                      "minimum": 0
                    },
                    {
                      "description": "Empty slot",
                      "type": "null"
                    }
                  ]
                }
              }
            },
            "result": {
              "type": "array",
              "items": {
                "type": "object",
                "additionalProperties": false,
                "required": [
                  "item_id",
                  "count"
                ],
                "properties": {
                  "item_id": {
                    "type": "integer",
                    "minimum": 0
                  },
                  "count": {
                    "type": "integer",
                    "minimum": 1
                  }
                }
              }
            }
          }
        },
        {
          "type": "object",
          "additionalProperties": false,
          "required": [
            "type",
            "ingredients",
            "result"
          ],
          "properties": {
            "type": {
              "const": "smelting"
            },
            "ingredients": {
              "type": "array",
              "maxItems": 1,
              "items": {
                "description": "Item ID",
                "type": "integer",
                "minimum": 0
              }
            },
            "result": {
              "type": "array",
              "items": {
                "type": "object",
                "additionalProperties": false,
                "required": [
                  "item_id",
                  "count"
                ],
                "properties": {
                  "item_id": {
                    "type": "integer",
                    "minimum": 0
                  },
                  "count": {
                    "type": "integer",
                    "minimum": 1
                  }
                }
              }
            }
          }
        }
      ]
    }
  }
}
```