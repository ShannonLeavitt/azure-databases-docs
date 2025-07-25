---
title: Computed properties
titleSuffix: Azure Cosmos DB for NoSQL
description: Computed properties in Azure Cosmos DB for NoSQL simplify complex queries and can increase query performance.
author: seesharprun
ms.author: sidandrews
ms.service: azure-cosmos-db
ms.subservice: nosql
ms.topic: reference
ms.devlang: nosql
ms.date: 06/11/2025
ms.custom: query-reference, sfi-image-nochange
---

# Computed properties in Azure Cosmos DB for NoSQL

[!INCLUDE[NoSQL](../../includes/appliesto-nosql.md)]

Computed properties in Azure Cosmos DB have values that are derived from existing item properties, but the properties aren't persisted in items themselves. Computed properties are scoped to a single item and can be referenced in queries as if they were persisted properties. Computed properties make it easier to write complex query logic once and reference it many times. You can add a single index on these properties or use them as part of a composite index for increased performance.

> [!NOTE]
> Do you have any feedback about computed properties? We want to hear it! Feel free to share feedback directly with the Azure Cosmos DB engineering team: [cosmoscomputedprops@microsoft.com](mailto:cosmoscomputedprops@microsoft.com).

## What is a computed property?

Computed properties must be at the top level in the item and can't have a nested path. Each computed property definition has two components: a name and a query. The name is the computed property name, and the query defines logic to calculate the property value for each item. Computed properties are scoped to an individual item and therefore can't use values from multiple items or rely on other computed properties. Every container can have a maximum of 20 computed properties.

Example computed property definition:

```json
{
  "computedProperties": [
    {
      "name": "cp_lowerName",
      "query": "SELECT VALUE LOWER(c.name) FROM c"
    }
  ]
}
```

### Name constraints

We strongly recommend that you name computed properties so that there's no collision with a persisted property name. To avoid overlapping property names, you can add a prefix or suffix to all computed property names. This article uses the prefix `cp_` in all name definitions.

> [!IMPORTANT]
> Defining a computed property by using the same name as a persisted property doesn't result in an error, but it might lead to unexpected behavior. Regardless of whether the computed property is indexed, values from persisted properties that share a name with a computed property won't be included in the index. Queries will always use the computed property instead of the persisted property, with the exception of the persisted property being returned instead of the computed property if there is a wildcard projection in the SELECT clause. Wildcard projection does not automatically include computed properties.

The constraints on computed property names are:

- All computed properties must have unique names.
- The value of the `name` property represents the top-level property name that can be used to reference the computed property.
- Reserved system property names such as `id`, `_rid`, and `_ts` can't be used as computed property names.
- A computed property name can't match a property path that is already indexed. This constraint applies to all indexing paths that are specified, including:
  - Included paths
  - Excluded paths
  - Spatial indexes
  - Composite indexes

### Query constraints

Queries in the computed property definition must be valid syntactically and semantically, otherwise the create or update operation fails. Queries should evaluate to a deterministic value for all items in a container. Queries might evaluate to undefined or null for some items, and computed properties with undefined or null values behave the same as persisted properties with undefined or null values when used in queries.

The limitations on computed property query definitions are:

- Queries must specify a FROM clause that represents the root item reference. Examples of supported FROM clauses are: `FROM c`, `FROM root c`, and `FROM MyContainer c`.
- Queries must use a VALUE clause in the projection.
- Queries can't include a JOIN.
- Queries can't use nondeterministic Scalar expressions. Examples of nondeterministic scalar expressions are: GetCurrentDateTime, GetCurrentTimeStamp, GetCurrentTicks, and RAND.
- Queries can't use any of the following clauses: WHERE, GROUP BY, ORDER BY, TOP, DISTINCT, OFFSET LIMIT, EXISTS, ALL, LAST, FIRST, and NONE.
- Queries can't include a scalar subquery.
- Aggregate functions, spatial functions, nondeterministic functions, and user defined functions (UDFs) aren't supported.

## Create computed properties

After the computed properties are created, you can execute queries that reference the properties by using any method, including all software development kits (SDKs) and Azure Data Explorer in the Azure portal.

| | Supported version | Notes |
| --- | --- | --- |
| **.NET SDK v3** | >= [3.34.0-preview](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.34.0-preview) | Computed properties are currently available only in preview package versions. |
| **Java SDK v4** | >= [4.46.0](https://mvnrepository.com/artifact/com.azure/azure-cosmos/4.46.0) | Computed properties are currently under preview version. |
| **Python SDK** | >= [v4.5.2b5](https://pypi.org/project/azure-cosmos/4.5.2b5/)  | Computed properties are currently under preview version. |

### Create computed properties by using the SDK

You can either create a new container that has computed properties defined, or you can add computed properties to an existing container.

Here's an example of how to create computed properties in a new container:

### [.NET](#tab/dotnet)

```csharp
ContainerProperties containerProperties = new ContainerProperties("myContainer", "/pk")
{
    ComputedProperties = new Collection<ComputedProperty>
    {
        new ComputedProperty
        {
            Name = "cp_lowerName",
            Query = "SELECT VALUE LOWER(c.name) FROM c"
        }
    }
};

Container container = await client.GetDatabase("myDatabase").CreateContainerAsync(containerProperties);
```

### [Java](#tab/java)

```java
 const { resource: contDefinition } = await containerWithComputedProperty.read();
  const upperName = {
    name: "upperLastName",
    query:
      "SELECT VALUE UPPER(IS_DEFINED(c.lastName) ? c.lastName : c.parents[0].familyName) FROM c",
  };
  if (contDefinition) {
    // update computed properties
    contDefinition.computedProperties = [upperName];
    // replace container definition with updated computed properties
    await containerWithComputedProperty.replace(contDefinition);
    console.log("Computed properties updated");
  } else {
    console.log("Container definition is undefined.");
  }
```

### [JavaScript](#tab/javascript)

```javascript
const lowerName = {
    name: "lowerLastName",
    query:
      "SELECT VALUE LOWER(IS_DEFINED(c.lastName) ? c.lastName : c.parents[0].familyName) FROM c",
};

const { container: containerWithComputedProperty } = await database.containers.createIfNotExists({
  id: containerId,
  computedProperties: [lowerName],
});
```

### [Python](#tab/python)

You can define multiple computed properties in a list and then add them to the container properties. Python SDK currently doesn't support computed properties on existing containers.

```python
computed_properties = [{'name': "cp_lower", 'query': "SELECT VALUE LOWER(c.db_group) FROM c"},
                       {'name': "cp_power", 'query': "SELECT VALUE POWER(c.val, 2) FROM c"},
                       {'name': "cp_str_len", 'query': "SELECT VALUE LENGTH(c.stringProperty) FROM c"}]

container_with_computed_props = db.create_container_if_not_exists(
            "myContainer", PartitionKey(path="/pk"), computed_properties=computed_properties)
```

Computed properties can be used like any other property in queries. For example, you can use the computed property `cp_lower` in a query like this:

```python
queried_items = list(
            container_with_computed_props.query_items(query='Select * from c Where c.cp_power = 25', partition_key="test"))
```

---

Here's an example of how to update computed properties on an existing container:

### [.NET](#tab/dotnet)

```csharp
var container = client.GetDatabase("myDatabase").GetContainer("myContainer");

// Read the current container properties
var containerProperties = await container.ReadContainerAsync();
// Make the necessary updates to the container properties
containerProperties.Resource.ComputedProperties = new Collection<ComputedProperty>
    {
        new ComputedProperty
        {
            Name = "cp_lowerName",
            Query = "SELECT VALUE LOWER(c.name) FROM c"
        },
        new ComputedProperty
        {
            Name = "cp_upperName",
            Query = "SELECT VALUE UPPER(c.name) FROM c"
        }
    };
// Update the container with changes
await container.ReplaceContainerAsync(containerProperties);
```

### [Java](#tab/java)

```java
CosmosContainer container = client.getDatabase("myDatabase").getContainer("myContainer");
// Read the current container properties
CosmosContainerProperties containerProperties = container.read().getProperties();
// Make the necessary updates to the container properties
Collection<ComputedProperty> modifiedComputedProperites = containerProperties.getComputedProperties();
modifiedComputedProperites.add(new ComputedProperty("cp_upperName", "SELECT VALUE UPPER(c.firstName) FROM c"));
containerProperties.setComputedProperties(modifiedComputedProperites);
// Update the container with changes
container.replace(containerProperties);
```

### [JavaScript](#tab/javascript)

```javascript
const { resource: contDefinition } = await containerWithComputedProperty.read();
const upperName = {
name: "upperLastName", query: "SELECT VALUE UPPER(IS_DEFINED(c.lastName) ? c.lastName : c.parents[0].familyName) FROM c",
};
if (contDefinition) {
// update computed properties contDefinition.computedProperties = [upperName]; // replace container definition with updated computed properties await containerWithComputedProperty.replace(contDefinition); console.log("Computed properties updated");
} else {
console.log("Container definition is undefined.");
}
```

### [Python](#tab/python)

Updating computed properties on an existing container isn't supported in Python SDK. You can only define computed properties when creating a new container.

---

> [!TIP]
> Every time you update container properties, the old values are overwritten. If you have existing computed properties and want to add new ones, be sure that you add both new and existing computed properties to the collection.

### Create computed properties by using the Data Explorer

You can use the Data Explorer to create a computed property for a container.

1. Open your existing container in the Data Explorer.

1. Navigate to the **Settings** section for your container. Then, navigate to the **Computed Properties* subsection.

1. Edit the computed properties definition JSON for your container. In this example, this JSON is used to define a computed property to split the `SKU` string for a retail product using the `-` delimiter.

    ```json
    [
      {
        "name": "cp_splitSku",
        "query": "SELECT VALUE StringSplit(p.sku, \"-\") FROM products p"
      }
    ]
    ```

    :::image type="content" source="media/computed-properties/data-explorer-editor.png" alt-text="Screenshot of the computed properties JSON editor in the Data Explorer interface.":::

1. **Save** the computed property.

## Use computed properties in queries

Computed properties can be referenced in queries the same way that persisted properties are referenced. Values for computed properties that aren't indexed are evaluated during runtime by using the computed property definition. If a computed property is indexed, the index is used the same way that it's used for persisted properties, and the computed property is evaluated on an as-needed basis. We recommend that you [add indexes on your computed properties](#index-computed-properties) for the best cost and performance.

The following examples use the quickstart products dataset that's available in [Data Explorer](../../data-explorer.md) in the Azure portal. To get started, select **Launch the quick start** and load the dataset in a new container.

:::image type="content" source="./media/computed-properties/data-explorer-home.png" alt-text="Screenshot illustrating how to load a sample data set into a database and container." border="false":::

Here's an example of an item:

```json
{
  "id": "aaaaaaaa-0000-1111-2222-bbbbbbbbbbbb",
  "categoryId": "bbbbbbbb-1111-2222-3333-cccccccccccc",
  "categoryName": "Bikes, Touring Bikes",
  "sku": "BK-T79U-50",
  "name": "Touring-1000 Blue, 50",
  "description": "The product called \"Touring-1000 Blue, 50\"",
  "price": 2384.07,
  "tags": [
    {
      "id": "cccccccc-2222-3333-4444-dddddddddddd",
      "name": "Tag-61"
    }
  ],
  "_rid": "n7AmAPTJ480GAAAAAAAAAA==",
  "_self": "dbs/n7AmAA==/colls/n7AmAPTJ480=/docs/n7AmAPTJ480GAAAAAAAAAA==/",
  "_etag": "\"01002683-0000-0800-0000-6451fb4b0000\"",
  "_attachments": "attachments/",
  "_ts": 1683094347
}
```

### Projection

If computed properties need to be projected, they must be explicitly referenced. Wildcard projections like `SELECT *` return all persisted properties, but they don't include computed properties.

Here's an example computed property definition to convert the `name` property to lowercase:

```json
{ 
  "name": "cp_lowerName", 
  "query": "SELECT VALUE LOWER(c.name) FROM c" 
} 
```

This property could then be projected in a query:

```nosql
SELECT 
    c.cp_lowerName 
FROM 
    c
```

### WHERE clause

Computed properties can be referenced in filter predicates like any persisted properties. We recommend that you add any relevant single or composite indexes when you use computed properties in filters.

Here's an example computed property definition to calculate a 20 percent price discount:

```json
{ 
  "name": "cp_20PercentDiscount", 
  "query": "SELECT VALUE (c.price * 0.2) FROM c" 
} 
```

This property could then be filtered on to ensure that only products where the discount would be less than $50 are returned:

```nosql
SELECT 
    c.price - c.cp_20PercentDiscount as discountedPrice, 
    c.name 
FROM 
    c 
WHERE 
    c.cp_20PercentDiscount < 50.00
```

### GROUP BY clause

As with persisted properties, computed properties can be referenced in the GROUP BY clause and use the index whenever possible. For the best performance, add any relevant single or composite indexes.

Here's an example computed property definition that finds the primary category for each item from the `categoryName` property:

```json
{
  "name": "cp_primaryCategory",
  "query": "SELECT VALUE SUBSTRING(c.categoryName, 0, INDEX_OF(c.categoryName, ',')) FROM c"
}
```

You can then group by `cp_primaryCategory` to get the count of items in each primary category:

```nosql
SELECT 
    COUNT(1), 
    c.cp_primaryCategory 
FROM 
    c 
GROUP BY 
    c.cp_primaryCategory
```

> [!TIP]
> Although you could also achieve this query without using computed properties, using the computed properties greatly simplifies writing the query and allows for increased performance because `cp_primaryCategory` can be indexed. Both [SUBSTRING()](./substring.md) and [INDEX_OF()](./index-of.md) require a [full scan](../../index-overview.md#index-usage) of all items in the container, but if you index the computed property, then the entire query can be served from the index instead. The ability to serve the query from the index instead of relying on a full scan increases performance and lowers query request unit (RU) costs.

### ORDER BY clause

As with persisted properties, computed properties can be referenced in the ORDER BY clause, and they must be indexed for the query to succeed. By using computed properties, you can ORDER BY the result of complex logic or system functions, which opens up many new query scenarios when you use Azure Cosmos DB.

Here's an example computed property definition that gets the month out of the `_ts` value:

```json
{
  "name": "cp_monthUpdated",
  "query": "SELECT VALUE DateTimePart('m', TimestampToDateTime(c._ts*1000)) FROM c"
}
```

Before you can ORDER BY `cp_monthUpdated`, you must add it to your indexing policy. After your indexing policy is updated, you can order by the computed property.

```nosql
SELECT
    *
FROM
    c
ORDER BY
    c.cp_monthUpdated
```

## Index computed properties

Computed properties aren't indexed by default and aren't covered by wildcard paths in the [indexing policy](../../index-policy.md). You can add single or composite indexes on computed properties in the indexing policy the same way you would add indexes on persisted properties. We recommend that you add relevant indexes to all computed properties. We recommend these indexes because they're beneficial in increasing performance and reducing request units (RUs). When computed properties are indexed, actual values are evaluated during item write operations to generate and persist index terms.

There are a few considerations for indexing computed properties, including:

- Computed properties can be specified in included paths, excluded paths, and composite index paths
- Computed properties can't have a spatial index defined on them
- Wildcard paths under the computed property path work like they do for regular properties
- Related indexes on a removed and indexed property must also be dropped

> [!NOTE]
> All computed properties are defined at the top level of the item. The path is always `/<computed property name>`.

> [!TIP]
> Every time you update container properties, the old values are overwritten. If you have existing computed properties and want to add new ones, be sure that you add both new and existing computed properties to the collection.

> [!NOTE]
> When the definition of an indexed computed property is modified, it's not automatically reindexed. To index the modified computed property, you'll first need to drop the computed property from the index. Then after the reindexing is completed, add the computed property back to the index policy.
>
> If you want to delete a computed property, you'll first need to remove it from the index policy.

### Add a single index for computed properties

To add a single index for a computed property named `cp_myComputedProperty`:

```json
{
  "indexingMode": "consistent",
  "automatic": true,
  "includedPaths": [
    {
      "path": "/*"
    },
    {
      "path": "/cp_myComputedProperty/?"
    }
  ],
  "excludedPaths": [
    {
      "path": "/\"_etag\"/?"
    }
  ]
}
```

### Add a composite index for computed properties

To add a composite index on two properties in which, one is computed as `cp_myComputedProperty`, and the other is persisted as `myPersistedProperty`:

```json
{
  "indexingMode": "consistent",
  "automatic": true,
  "includedPaths": [
    {
      "path": "/*"
    }
  ],
  "excludedPaths": [
    {
      "path": "/\"_etag\"/?"
    }
  ],
  "compositeIndexes": [
    [
      {
        "path": "/cp_myComputedProperty"
      },
      {
        "path": "/path/to/myPersistedProperty"
      }
    ]
  ]
}
```

## Understand request unit consumption

Adding computed properties to a container doesn't consume RUs. Write operations on containers that have computed properties defined might have a slight RU increase. If a computed property is indexed, RUs on write operations increase to reflect the costs for indexing and evaluation of the computed property.

## Related content

- [Manage indexing policies](../how-to-manage-indexing-policy.md)
- [Model document data](../../modeling-data.md)
