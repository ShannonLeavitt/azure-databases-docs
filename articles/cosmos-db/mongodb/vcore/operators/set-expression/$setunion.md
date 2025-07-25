---
  title: $setUnion (set expression)
  titleSuffix: Azure Cosmos DB for MongoDB vCore
  description: The $setUnion operator returns an array containing all unique elements from the input arrays.
  author: avijitgupta
  ms.author: avijitgupta
  ms.service: azure-cosmos-db
  ms.subservice: mongodb-vcore
  ms.topic: language-reference
  ms.date: 06/09/2025
---

# $setUnion (set expression)

[!INCLUDE[MongoDB (vCore)](~/reusable-content/ce-skilling/azure/includes/cosmos-db/includes/appliesto-mongodb-vcore.md)]

The `$setUnion` operator returns an array containing all unique elements from the input arrays. It treats arrays as sets, removing duplicates and ignoring element order. The result contains each unique element only once, regardless of how many times it appears across the input arrays.

## Syntax

The syntax for the `$setUnion` operator is as follows:

```javascript
{
  $setUnion: [ <array1>, <array2>, ... ]
}
```

## Parameters

| | Description |
| --- | --- |
| **`<array1>, <array2>, ...`** | Two or more arrays to be combined. Each array is treated as a set, and duplicates are removed from the final result. |

## Example

Let's understand the usage with sample json from `stores` dataset.

```json
{
  "_id": "26afb024-53c7-4e94-988c-5eede72277d5",
  "name": "First Up Consultants | Microphone Bazaar - South Lexusland",
  "location": {
    "lat": -29.1866,
    "lon": -112.7858
  },
  "staff": {
    "totalStaff": {
      "fullTime": 14,
      "partTime": 8
    }
  },
  "sales": {
    "totalSales": 83865,
    "salesByCategory": [
      {
        "categoryName": "Lavalier Microphones",
        "totalSales": 44174
      },
      {
        "categoryName": "Wireless Microphones",
        "totalSales": 39691
      }
    ]
  },
  "promotionEvents": [
    {
      "eventName": "Price Cut Spectacular",
      "promotionalDates": {
        "startDate": {
          "Year": 2023,
          "Month": 12,
          "Day": 26
        },
        "endDate": {
          "Year": 2024,
          "Month": 1,
          "Day": 5
        }
      },
      "discounts": [
        {
          "categoryName": "Condenser Microphones",
          "discountPercentage": 5
        },
        {
          "categoryName": "Dynamic Microphones",
          "discountPercentage": 14
        }
      ]
    },
    {
      "eventName": "Bargain Bonanza",
      "promotionalDates": {
        "startDate": {
          "Year": 2024,
          "Month": 3,
          "Day": 25
        },
        "endDate": {
          "Year": 2024,
          "Month": 4,
          "Day": 3
        }
      },
      "discounts": [
        {
          "categoryName": "Streaming Microphones",
          "discountPercentage": 14
        },
        {
          "categoryName": "Microphone Stands",
          "discountPercentage": 14
        }
      ]
    },
    {
      "eventName": "Super Savings Extravaganza",
      "promotionalDates": {
        "startDate": {
          "Year": 2024,
          "Month": 6,
          "Day": 23
        },
        "endDate": {
          "Year": 2024,
          "Month": 6,
          "Day": 30
        }
      },
      "discounts": [
        {
          "categoryName": "Condenser Microphones",
          "discountPercentage": 7
        },
        {
          "categoryName": "Microphone Stands",
          "discountPercentage": 5
        }
      ]
    }
  ]
}
```

### Example 1: Combine all product categories

The example gets a complete list of all unique product categories that a store deals with, including both sales and promotion categories.

```javascript
db.stores.aggregate([
  { $match: {"_id": "26afb024-53c7-4e94-988c-5eede72277d5"} },
  {
    $project: {
      name: 1,
      salesCategories: "$sales.salesByCategory.categoryName",
      firstPromotionCategories: { $arrayElemAt: ["$promotionEvents.discounts.categoryName", 0] },
      secondPromotionCategories: { $arrayElemAt: ["$promotionEvents.discounts.categoryName", 1] },
      thirdPromotionCategories: { $arrayElemAt: ["$promotionEvents.discounts.categoryName", 2] },
      allUniqueCategories: {
        $setUnion: [
          "$sales.salesByCategory.categoryName",
          { $arrayElemAt: ["$promotionEvents.discounts.categoryName", 0] },
          { $arrayElemAt: ["$promotionEvents.discounts.categoryName", 1] },
          { $arrayElemAt: ["$promotionEvents.discounts.categoryName", 2] }
        ]
      }
    }
  }
])
```

The query returns all the unique categories across sales and promotions.

```json
{
  "_id": "26afb024-53c7-4e94-988c-5eede72277d5",
  "name": "First Up Consultants | Microphone Bazaar - South Lexusland",
  "salesCategories": [
    "Lavalier Microphones",
    "Wireless Microphones"
  ],
  "firstPromotionCategories": [
    "Condenser Microphones",
    "Dynamic Microphones"
  ],
  "secondPromotionCategories": [
    "Streaming Microphones",
    "Microphone Stands"
  ],
  "thirdPromotionCategories": [
    "Condenser Microphones",
    "Microphone Stands"
  ],
  "allUniqueCategories": [
    "Lavalier Microphones",
    "Wireless Microphones",
    "Condenser Microphones",
    "Dynamic Microphones",
    "Streaming Microphones",
    "Microphone Stands"
  ]
}
```

## Related content

[!INCLUDE[Related content](../includes/related-content.md)]