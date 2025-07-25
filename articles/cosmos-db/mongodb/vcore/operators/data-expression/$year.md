---
  title: $year (date expression)
  titleSuffix: Azure Cosmos DB for MongoDB vCore
  description: The $year operator returns the year for a date as a four-digit number.
  author: avijitgupta
  ms.author: avijitgupta
  ms.service: azure-cosmos-db
  ms.subservice: mongodb-vcore
  ms.topic: reference
  ms.date: 06/20/2025
---

# $year (date expression)

[!INCLUDE[MongoDB (vCore)](~/reusable-content/ce-skilling/azure/includes/cosmos-db/includes/appliesto-mongodb-vcore.md)]

The `$year` operator returns the year for a date as a four-digit number (e.g., 2024). If the date is null or missing, `$year` returns null.

## Syntax

The syntax for the `$year` operator is as follows:

```javascript
{
  $year: <dateExpression>
}
```

Or with timezone specification:

```javascript
{
  $year: {
    date: <dateExpression>,
    timezone: <timezoneExpression>
  }
}
```

## Parameters

| | Description |
| --- | --- |
| **`dateExpression`** | Any expression that resolves to a Date, Timestamp, or ObjectId. |
| **`timezone`** | Optional. The timezone to use for the calculation. Can be an Olson Timezone Identifier (e.g., "America/New_York") or a UTC offset (e.g., "+0530"). |

## Example

Let's understand the usage with sample JSON from the `stores` dataset.

```json
{
  "_id": "905d1939-e03a-413e-a9c4-221f74055aac",
  "name": "Trey Research | Home Office Depot - Lake Freeda",
  "location": { "lat": -48.9752, "lon": -141.6816 },
  "staff": { "employeeCount": { "fullTime": 12, "partTime": 19 } },
  "sales": {
    "salesByCategory": [ { "categoryName": "Desk Lamps", "totalSales": 37978 } ],
    "revenue": 37978
  },
  "promotionEvents": [
    {
      "eventName": "Crazy Deal Days",
      "promotionalDates": {
        "startDate": { "Year": 2023, "Month": 9, "Day": 27 },
        "endDate": { "Year": 2023, "Month": 10, "Day": 4 }
      },
      "discounts": [
        { "categoryName": "Desks", "discountPercentage": 22 },
        { "categoryName": "Filing Cabinets", "discountPercentage": 23 }
      ]
    }
  ],
  "company": "Trey Research",
  "city": "Lake Freeda",
  "storeOpeningDate": "2024-12-30T22:55:25.779Z",
  "lastUpdated": { "t": 1729983325, "i": 1 }
}
```

### Example 1: Extract year from store opening date

The example extracts the year when the store was opened.

```javascript
db.stores.aggregate([
  { $match: { "_id": "905d1939-e03a-413e-a9c4-221f74055aac" } },
  {
    $project: {
      name: 1,
      storeOpeningDate: 1,
      openingYear: { $year: { $toDate: "$storeOpeningDate" } }
    }
  }
])
```

The query returns the year the store was opened.

```json
{
  "_id": "905d1939-e03a-413e-a9c4-221f74055aac",
  "name": "Trey Research | Home Office Depot - Lake Freeda",
  "storeOpeningDate": "2024-12-30T22:55:25.779Z",
  "openingYear": 2024
}
```

### Example 2: Find stores opened in specific year

This example finds all stores that were opened in a specific year.

```javascript
db.stores.aggregate([
  {
    $match: {
      $expr: {
        $eq: [{ $year: { $toDate: "$storeOpeningDate" } }, 2021]
      }
    }
  },
  {
    $project: {
      name: 1,
      city: 1,
      openingYear: { $year: { $toDate: "$storeOpeningDate" } },
      storeOpeningDate: 1
    }
  }
])
```

The query returns stores opened in 2021.

```json
{
  "_id": "2cf3f885-9962-4b67-a172-aa9039e9ae2f",
  "city": "South Amir",
  "storeOpeningDate": "2021-10-03T00:00:00.000Z",
  "name": "First Up Consultants | Bed and Bath Center - South Amir",
  "openingYear": 2021
}
```

## Related content

[!INCLUDE[Related content](../includes/related-content.md)]
