---
author: nachoalonsoportillo
ms.author: ialonso
ms.reviewer: maghan
ms.date: 05/05/2025
ms.service: azure-database-postgresql
ms.subservice: flexible-server
ms.topic: include
ms.custom: automatically generated
---
### autovacuum_work_mem

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Sets the maximum memory to be used by each autovacuum worker process. |
| Data type | integer |
| Default value | `-1` |
| Allowed values | `-1-2097151` |
| Parameter type | dynamic |
| Documentation | [autovacuum_work_mem](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-AUTOVACUUM-WORK-MEM) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### commit_timestamp_buffers

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Specifies the amount of memory to use to cache the contents of pg_commit_ts. Unit is 8kb. |
| Data type | integer |
| Default value | `1024` |
| Allowed values | `0-131072` |
| Parameter type | static |
| Documentation | [commit_timestamp_buffers](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-COMMIT_TIMESTAMP_BUFFERS) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### dynamic_shared_memory_type

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Selects the dynamic shared memory implementation used. |
| Data type | enumeration |
| Default value | `posix` |
| Allowed values | `posix` |
| Parameter type | read-only |
| Documentation | [dynamic_shared_memory_type](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-DYNAMIC-SHARED-MEMORY-TYPE) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### hash_mem_multiplier

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Multiple of work_mem to use for hash tables. |
| Data type | numeric |
| Default value | `2` |
| Allowed values | `1-1000` |
| Parameter type | dynamic |
| Documentation | [hash_mem_multiplier](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-HASH-MEM-MULTIPLIER) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### huge_pages

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Enables/disables the use of huge memory pages. This setting is not applicable to servers having less than 4 vCores. |
| Data type | enumeration |
| Default value | `try` |
| Allowed values | `on,off,try` |
| Parameter type | static |
| Documentation | [huge_pages](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-HUGE-PAGES) |


[!INCLUDE [server-parameters-azure-notes-huge-pages](./server-parameters-azure-notes-huge-pages.md)]



### huge_page_size

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | The size of huge page that should be requested. |
| Data type | integer |
| Default value | `0` |
| Allowed values | `0` |
| Parameter type | read-only |
| Documentation | [huge_page_size](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-HUGE-PAGE-SIZE) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### logical_decoding_work_mem

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Sets the maximum memory to be used for logical decoding. |
| Data type | integer |
| Default value | `65536` |
| Allowed values | `64-2147483647` |
| Parameter type | dynamic |
| Documentation | [logical_decoding_work_mem](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-LOGICAL-DECODING-WORK-MEM) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### maintenance_work_mem

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Sets the maximum memory to be used for maintenance operations such as VACUUM, Create Index. |
| Data type | integer |
| Default value | Depends on resources (vCores, RAM, or disk space) allocated to the server. |
| Allowed values | `1024-2097151` |
| Parameter type | dynamic |
| Documentation | [maintenance_work_mem](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-MAINTENANCE-WORK-MEM) |


[!INCLUDE [server-parameters-azure-notes-maintenance-work-mem](./server-parameters-azure-notes-maintenance-work-mem.md)]



### max_prepared_transactions

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Sets the maximum number of simultaneously prepared transactions. When running a replica server, you must set this parameter to the same or higher value than on the primary server. |
| Data type | integer |
| Default value | `0` |
| Allowed values | `0-262143` |
| Parameter type | static |
| Documentation | [max_prepared_transactions](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-MAX-PREPARED-TRANSACTIONS) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### max_stack_depth

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Sets the maximum stack depth, in kilobytes. |
| Data type | integer |
| Default value | `2048` |
| Allowed values | `2048` |
| Parameter type | read-only |
| Documentation | [max_stack_depth](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-MAX-STACK-DEPTH) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### min_dynamic_shared_memory

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Amount of dynamic shared memory reserved at startup. |
| Data type | integer |
| Default value | `0` |
| Allowed values | `0` |
| Parameter type | read-only |
| Documentation | [min_dynamic_shared_memory](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-MIN-DYNAMIC-SHARED-MEMORY) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### multixact_member_buffers

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Specifies the amount of shared memory to use to cache the contents of pg_multixact/members. Unit is 8kb. |
| Data type | integer |
| Default value | `32` |
| Allowed values | `16-131072` |
| Parameter type | static |
| Documentation | [multixact_member_buffers](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-MUTIXACT_MEMBER_BUFFERS) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### multixact_offset_buffers

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Specifies the amount of shared memory to use to cache the contents of pg_multixact/offsets. Unit is 8kb. |
| Data type | integer |
| Default value | `16` |
| Allowed values | `16-131072` |
| Parameter type | static |
| Documentation | [multixact_offset_buffers](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-MULTIXACT_OFFSET_BUFFERS) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### notify_buffers

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Specifies the amount of shared memory to use to cache the contents of pg_notify. Unit is 8kb. |
| Data type | integer |
| Default value | `16` |
| Allowed values | `16-131072` |
| Parameter type | static |
| Documentation | [notify_buffers](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-NOTIFY_BUFFERS) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### serializable_buffers

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Specifies the amount of shared memory to use to cache the contents of pg_serial. Unit is 8kb. |
| Data type | integer |
| Default value | `32` |
| Allowed values | `16-131072` |
| Parameter type | static |
| Documentation | [serializable_buffers](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-SERIALIZABLE_BUFFERS) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### shared_buffers

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Sets the number of shared memory buffers used by the server. Unit is 8kb. Allowed values are inside the range of 10% - 75% of available memory. |
| Data type | integer |
| Default value | Depends on resources (vCores, RAM, or disk space) allocated to the server. |
| Allowed values | `16-1073741823` |
| Parameter type | static |
| Documentation | [shared_buffers](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-SHARED-BUFFERS) |


[!INCLUDE [server-parameters-azure-notes-shared-buffers](./server-parameters-azure-notes-shared-buffers.md)]



### shared_memory_type

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Selects the shared memory implementation used for the main shared memory region. |
| Data type | enumeration |
| Default value | `mmap` |
| Allowed values | `mmap` |
| Parameter type | read-only |
| Documentation | [shared_memory_type](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-SHARED-MEMORY-TYPE) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### subtransaction_buffers

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Specifies the amount of shared memory to use to cache the contents of pg_subtrans. Unit is 8kb. |
| Data type | integer |
| Default value | `1024` |
| Allowed values | `0-131072` |
| Parameter type | static |
| Documentation | [subtransaction_buffers](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-SUBTRANSACTION_BUFFERS) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### temp_buffers

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Sets the maximum number of temporary buffers used by each database session. |
| Data type | integer |
| Default value | `1024` |
| Allowed values | `100-1073741823` |
| Parameter type | dynamic |
| Documentation | [temp_buffers](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-TEMP-BUFFERS) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### transaction_buffers

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Specifies the amount of shared memory to use to cache the contents of pg_xact. Unit is 8kb. |
| Data type | integer |
| Default value | `1024` |
| Allowed values | `0-131072` |
| Parameter type | static |
| Documentation | [transaction_buffers](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-TRANSACTION_BUFFERS) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### vacuum_buffer_usage_limit

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Sets the buffer pool size for VACUUM, ANALYZE, and autovacuum. |
| Data type | integer |
| Default value | `2048` |
| Allowed values | `0-16777216` |
| Parameter type | dynamic |
| Documentation | [vacuum_buffer_usage_limit](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-VACUUM-BUFFER-USAGE-LIMIT) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### work_mem

| Attribute | Value |
| --- | --- |
| Category | Resource Usage / Memory |
| Description | Sets the amount of memory to be used by internal sort operations and hash tables before writing to temporary disk files. |
| Data type | integer |
| Default value | `4096` |
| Allowed values | `4096-2097151` |
| Parameter type | dynamic |
| Documentation | [work_mem](https://www.postgresql.org/docs/17/runtime-config-resource.html#GUC-WORK-MEM) |


[!INCLUDE [server-parameters-azure-notes-work-mem](./server-parameters-azure-notes-work-mem.md)]



