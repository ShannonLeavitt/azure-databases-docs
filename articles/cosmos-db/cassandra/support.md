---
title: Apache Cassandra features supported by Azure Cosmos DB for Apache Cassandra
description: Learn about the Apache Cassandra feature support in Azure Cosmos DB for Apache Cassandra and the benefits of the Apache Cassandra APIs.
author: IriaOsara
ms.author: iriaosara
ms.service: azure-cosmos-db
ms.subservice: apache-cassandra
ms.topic: overview
ms.date: 06/06/2025
#customer intent: As a database administrator, I want to understand the Apache Cassandra features supported by Azure Cosmos DB for Apache Cassandra.
---

# Apache Cassandra features supported by Azure Cosmos DB for Apache Cassandra

[!INCLUDE[Cassandra](../includes/appliesto-cassandra.md)]

Azure Cosmos DB is Microsoft's globally distributed multi-model database service. You can communicate with the Azure Cosmos DB for Apache Cassandra through the Cassandra Query Language (CQL) Binary Protocol v4 [wire protocol](https://github.com/apache/cassandra/blob/trunk/doc/native_protocol_v4.spec) compliant open-source Cassandra client [drivers](https://cassandra.apache.org/doc/stable/cassandra/getting-started/drivers.html).

By using the Azure Cosmos DB for Apache Cassandra, you can enjoy the benefits of the Apache Cassandra APIs and the enterprise capabilities that Azure Cosmos DB provides. The enterprise capabilities include [global distribution](../distribute-data-globally.md), [automatic scale out partitioning](partitioning.md), availability and latency guarantees, encryption at rest, and backups.

## Cassandra protocol 

The Azure Cosmos DB for Apache Cassandra is compatible with Cassandra Query Language (CQL) v3.11 API. It's backward-compatible with version 2.x. The supported CQL commands, tools, limitations, and exceptions are listed later in this article. Any client driver that understands these protocols should be able to connect to Azure Cosmos DB for Apache Cassandra.

## Azure Managed Instance for Apache Cassandra

For some customers, adapting to API for Cassandra can be a challenge due to differences in behavior and configuration, especially for lift-and-shift migrations. If a feature that is critical for your application is listed as not supported later in this article, consider using [Azure Managed Instance for Apache Cassandra](../../managed-instance-apache-cassandra/introduction.md). This service is a first-party Azure service for hosting and maintaining pure open-source Apache Cassandra clusters with 100% compatibility.

## Cassandra driver

Azure Cosmos DB for Apache Cassandra supports the following versions of Cassandra drivers:

- [Java 3.5+](https://github.com/datastax/java-driver)  
- [C# 3.5+](https://github.com/datastax/csharp-driver)  
- [Nodejs 3.5+](https://github.com/datastax/nodejs-driver)  
- [Python 3.15+](https://github.com/datastax/python-driver)  
- [C++ 2.9](https://github.com/datastax/cpp-driver)   
- [PHP 1.3](https://github.com/datastax/php-driver)  
- [Gocql](https://github.com/gocql/gocql)  

## CQL data types 

Azure Cosmos DB for Apache Cassandra supports the following CQL data types:

|Type  |Supported |
|---------|---------|
| `ascii`  | Yes |
| `bigint`  | Yes |
| `blob`  | Yes |
| `boolean`  | Yes |
| `counter` | Yes |
| `date`  | Yes |
| `decimal`  | Yes |
| `double`  | Yes |
| `float`  | Yes |
| `frozen`  | Yes |
| `inet`  | Yes |
| `int`  | Yes |
| `list`  | Yes |
| `set`  | Yes |
| `smallint`  | Yes |
| `text`  | Yes |
| `time`  | Yes |
| `timestamp`  | Yes |
| `timeuuid`  | Yes |
| `tinyint`  | Yes |
| `tuple`  | Yes |
| `uuid`  | Yes |
| `varchar`  | Yes |
| `varint`  | Yes |
| `tuples` | Yes | 
| `udts`  | Yes |
| `map` | Yes |

Static is supported for data type declaration.

## CQL functions

Azure Cosmos DB for Apache Cassandra supports the following CQL functions:

|Command  |Supported |
|---------|---------|
| `Token` * | Yes |
| `ttl` *** | Yes |
| `writetime` *** | Yes |
| `cast` ** | Yes |

> [!NOTE] 
> \* API for Cassandra supports token as a projection/selector, and only allows token(pk) on the left-hand side of a where clause. For example, `WHERE token(pk) > 1024` is supported, but `WHERE token(pk) > token(100)` is *not* supported.
> \*\* The `cast()` function isn't nestable in API for Cassandra. For example, `SELECT cast(count as double) FROM myTable` is supported, but `SELECT avg(cast(count as double)) FROM myTable` is *not* supported.
> \*\*\* Custom time stamps and TTL specified with the `USING` option are applied at a row level (and not per cell).

Aggregate functions:

|Command  |Supported |
|---------|---------|
| `avg` | Yes |
| `count` | Yes |
| `min` | Yes |
| `max` | Yes |
| `sum` | Yes |

> [!NOTE]
> Aggregate functions work on regular columns, but aggregates on clustering columns are *not* supported.


Blob conversion functions:
 
|Command  |Supported |
|---------|---------|
| `typeAsBlob(value)`   | Yes |
| `blobAsType(value)` | Yes |


UUID and timeuuid functions:
 
|Command  |Supported |
|---------|---------|
| `dateOf()`  | Yes |
| `now()`  | Yes |
| `minTimeuuid()`  | Yes |
| `unixTimestampOf()`  | Yes |
| `toDate(timeuuid)`  | Yes |
| `toTimestamp(timeuuid)`  | Yes |
| `toUnixTimestamp(timeuuid)`  | Yes |
| `toDate(timestamp)`  | Yes |
| `toUnixTimestamp(timestamp)`  | Yes |
| `toTimestamp(date)`  | Yes |
| `toUnixTimestamp(date)` | Yes |

## CQL commands

Azure Cosmos DB supports the following database commands on API for Cassandra accounts.

|Command  |Supported |
|---------|---------|
| `ALLOW FILTERING` | Yes |
| `ALTER KEYSPACE` | N/A (PaaS service, replication managed internally)|
| `ALTER MATERIALIZED VIEW` | Yes |
| `ALTER ROLE` | No |
| `ALTER TABLE` | Yes |
| `ALTER TYPE` | No |
| `ALTER USER` | No |
| `BATCH` | Yes (unlogged batch only)|
| `COMPACT STORAGE` | N/A (PaaS service) |
| `CREATE AGGREGATE` | No | 
| `CREATE CUSTOM INDEX (SASI)` | No |
| `CREATE INDEX` | Yes (including [named indexes](secondary-indexing.md) but full FROZEN collection isn't supported) |
| `CREATE FUNCTION` | No |
| `CREATE KEYSPACE` (replication settings ignored) | Yes |
| `CREATE MATERIALIZED VIEW` | Yes |
| `CREATE TABLE` | Yes |
| `CREATE TRIGGER` | No |
| `CREATE TYPE` | Yes |
| `CREATE ROLE` | No |
| `CREATE USER` (Deprecated in native Apache Cassandra) | No |
| `DELETE` | Yes |
| `DISTINCT` | No |
| `DROP AGGREGATE` | No |
| `DROP FUNCTION` | No |
| `DROP INDEX` | Yes |
| `DROP KEYSPACE` | Yes |
| `DROP MATERIALIZED VIEW` | Yes |
| `DROP ROLE` | No |
| `DROP TABLE` | Yes |
| `DROP TRIGGER` | No | 
| `DROP TYPE` | Yes |
| `DROP USER` (Deprecated in native Apache Cassandra) | No |
| `GRANT` | No |
| `INSERT` | Yes |
| `LIST PERMISSIONS` | No |
| `LIST ROLES` | No |
| `LIST USERS` (Deprecated in native Apache Cassandra) | No |
| `REVOKE` | No |
| `SELECT` | Yes |
| `UPDATE` | Yes |
| `TRUNCATE` | Yes |
| `USE` | Yes |

## Lightweight Transactions (LWT)

| Component  |Supported |
|---------|---------|
| `DELETE IF EXISTS` | Yes |
| `DELETE conditions` | Yes |
| `INSERT IF NOT EXISTS` | Yes |
| `UPDATE IF EXISTS` | Yes |
| `UPDATE IF NOT EXISTS` | Yes |
| `UPDATE conditions` | Yes |

> [!NOTE]
> Lightweight transactions currently aren't supported for accounts that have multiple region writes enabled.

## CQL Shell commands

Azure Cosmos DB supports the following database commands on API for Cassandra accounts.

|Command  |Supported |
|---------|---------|
| `CAPTURE` | Yes |
| `CLEAR` | Yes |
| `CONSISTENCY` * | N/A |
| `COPY` | No |
| `DESCRIBE` | Yes |
| `cqlshExpand` | No |
| `EXIT` | Yes |
| `LOGIN` | N/A (CQL function `USER` isn't supported, hence `LOGIN` is redundant) |
| `PAGING` | Yes |
| `SERIAL CONSISTENCY` * | N/A |
| `SHOW` | Yes |
| `SOURCE` | Yes |
| `TRACING` | N/A (API for Cassandra is backed by Azure Cosmos DB - use [diagnostic logging](../monitor-resource-logs.md) for troubleshooting) |

> [!NOTE] 
> Consistency works differently in Azure Cosmos DB. For more information, see [Apache Cassandra and Azure Cosmos DB for Apache Cassandra consistency levels](consistency-mapping.md).

## JSON Support
|Command  |Supported |
|---------|---------|
| `SELECT JSON` | Yes |
| `INSERT JSON` | Yes |
| `fromJson()` | No |
| `toJson()` | No |

## API for Cassandra limits

Azure Cosmos DB for Apache Cassandra doesn't have any limits on the size of data stored in a table. Hundreds of terabytes or Petabytes of data can be stored while ensuring partition key limits are honored. Similarly, every entity or row equivalent doesn't have any limits on the number of columns. The total size of the entity shouldn't exceed 2 MB. The data per partition key can't exceed 20 GB as in all other APIs.

## Tools 

Azure Cosmos DB for Apache Cassandra is a managed service platform. The platform doesn't require any management overhead or utilities such as Garbage Collector, Java Virtual Machine (JVM), and node tool to manage the cluster. Tools such as `cqlsh` that use Binary CQLv4 compatibility are supported. 

- The Azure portal's data explorer, metrics, log diagnostics, PowerShell, and CLI are other supported mechanisms to manage the account.

## CQL shell

You can connect to the API for Cassandra in Azure Cosmos DB by using the CQLSH installed on a local machine. It comes with Apache Cassandra 3.11 and works out of the box by setting the environment variables. The following sections include the instructions to install, configure, and connect to API for Cassandra in Azure Cosmos DB, on Windows or Linux using CQLSH.

> [!WARNING]
> Connections to Azure Cosmos DB for Apache Cassandra don't work with DataStax Enterprise (DSE) or Cassandra 4.0 versions of CQLSH. Ensure that you use only v3.11 open source Apache Cassandra versions of CQLSH when connecting to API for Cassandra. 

Windows:

1. Install [Python 3](https://www.python.org/downloads/windows/).
1. Install PIP.

   1. Before you install PIP, download the *get-pip.py* file.
   1. Launch a command prompt window, if it isn't already open. To do so, open the Windows search bar, type *cmd* and select the icon.
   1. Then, run the following command to download the *get-pip.py* file:

   ```bash
   curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py 
   ```

1. Install PIP on Windows:

   ```bash
   python get-pip.py
   ```

1. Verify the PIP installation. Look for a message from step 3 to confirm which folder PIP was installed in. Then navigate to that folder and run the command pip help.

1. Install CQLSH using PIP:

   ```bash
   pip3 install cqlsh==5.0.3
   ```
1. Install [Python 2](https://www.python.org/downloads/windows/).
1. Run the [CQLSH using the authentication mechanism](manage-data-cqlsh.md#update-your-connection-string).

> [!NOTE]
> You need to set the environment variables to point to  the Python 2 folder.

Install on Unix/Linux/Mac:

```bash
# Install default-jre and default-jdk
sudo apt install default-jre
sudo apt-get update
sudo apt install default-jdk

# Import the Baltimore CyberTrust root certificate:
curl https://cacert.omniroot.com/bc2025.crt > bc2025.crt
keytool -importcert -alias bc2025ca -file bc2025.crt

# Install the Cassandra libraries in order to get CQLSH:
echo "deb https://downloads.apache.org/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -
sudo apt-get update
sudo apt-get install cassandra=3.11.13
```

Connect with Unix/Linux/Mac:

```bash
# Export the SSL variables:
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false

# Connect to Azure Cosmos DB for Apache Cassandra:
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl --protocol-version=4
```

Connect with Docker:

```bash
docker run -it --rm -e SSL_VALIDATE=false -e SSL_VERSION=TLSv1_2 cassandra:3.11 cqlsh <account_name>.cassandra.cosmos.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl
```

All CRUD operations that are run through a CQL v4 compatible SDK return extra information about error and request units consumed. To ensure the most efficient use of the provisioned throughput, the DELETE and UPDATE commands should be handled with resource governance taken into consideration.

> [!NOTE]
> The `gc_grace_seconds` value must be zero, if specified.

```csharp
var tableInsertStatement = table.Insert(sampleEntity); 
var insertResult = await tableInsertStatement.ExecuteAsync(); 
 
foreach (string key in insertResult.Info.IncomingPayload) 
        { 
            byte[] valueInBytes = customPayload[key]; 
            double value = Encoding.UTF8.GetString(valueInBytes); 
            Console.WriteLine($"CustomPayload:  {key}: {value}"); 
        } 
```

## Consistency mapping 

Azure Cosmos DB for Apache Cassandra provides choice of consistency for read operations. For more information, see [Mapping consistency levels](consistency-mapping.md#mapping-consistency-levels).

## Permission and role management

Azure Cosmos DB supports Azure role-based access control (Azure RBAC) for provisioning, rotating keys, viewing metrics and read-write and read-only passwords/keys that can be obtained through the [Azure portal](https://portal.azure.com). Azure Cosmos DB doesn't support roles for CRUD activities.

## Keyspace and Table options

The options for region name, class, replication_factor, and datacenter in the `CREATE KEYSPACE` command are ignored currently. The system uses the underlying Azure Cosmos DB's [global distribution](../global-dist-under-the-hood.md) replication method to add the regions. If you need the cross-region presence of data, you can enable it at the account level with PowerShell, CLI, or the Azure portal. For more information, see [Add regions to your database account](../how-to-manage-database-account.yml#add-remove-regions-from-your-database-account).

Durable_writes can't be disabled because Azure Cosmos DB ensures every write is durable. In every region, Azure Cosmos DB replicates the data across the replica set that is made up of four replicas and this replica set [configuration](../global-dist-under-the-hood.md) can't be modified.
 
All the options are ignored when creating the table, except `gc_grace_seconds`, which should be set to zero.
The Keyspace and table have an extra option named `cosmosdb_provisioned_throughput` with a minimum value of 400 RU/s. The Keyspace throughput allows sharing throughput across multiple tables. It's useful for scenarios when all tables aren't using the provisioned throughput. The `ALTER TABLE` command allows changing the provisioned throughput across the regions. 

```sql
CREATE KEYSPACE sampleks WITH REPLICATION = {  'class' : 'SimpleStrategy'} AND cosmosdb_provisioned_throughput=2000;  

CREATE TABLE sampleks.t1(user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=2000; 

ALTER TABLE gks1.t1 WITH cosmosdb_provisioned_throughput=10000 ;

```

## Secondary Index

API for Cassandra supports secondary indexes on all data types except frozen collection types, decimal, and variant types. 

## Usage of Cassandra retry connection policy

Azure Cosmos DB is a resource governed system. You can do some operations in a given second based on the request units consumed by the operations. If an application exceeds that limit in a given second, requests are rate-limited and exceptions are thrown. The API for Cassandra in Azure Cosmos DB translates these exceptions to overloaded errors on the Cassandra native protocol.

To ensure that your application can intercept and retry requests if there's rate limitation, the [spark](https://mvnrepository.com/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper) and the [Java](https://github.com/Azure/azure-cosmos-cassandra-extensions) extensions are provided. See also Java code samples for [version 3](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample) and [version 4](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample-v4) Datastax drivers, when connecting to API for Cassandra in Azure Cosmos DB. If you use other SDKs to access API for Cassandra in Azure Cosmos DB, create a retry policy to retry on these exceptions. Alternatively, [enable server-side retries](prevent-rate-limiting-errors.md) for API for Cassandra. 

## Next steps

- Get started with [creating an API for Cassandra account, database, and a table](create-account-java.md) by using a Java application
