# Overview
Monitors an OpenSearch cluster using their API's for various performance, and availability metrics.

# Setup
On OpenSearch, create a user for Zabbix to monitor with. This user will need the following permissions:

**Cluster permissions**
 - `cluster:monitor/*`

**Index permissions**
 - `indices:monitor/*`
 
This template only uses [HTTP Agent](https://www.zabbix.com/documentation/current/en/manual/config/items/itemtypes/http) requests and the API calls will come from your Zabbix Server or proxy to your OpenSearch node. Ensure all relevant firewall rules are in place to allow that traffic.

On your Zabbix host, configure at a minimum the following Macros:
 - `{$OS.USERNAME}`
 - `{$OS.PASSWORD}`
 - `{$OS.CLUSTER.HOSTNAME}`

# Macros used

| Name                    | Description                                                                                                        | Default   |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------ | --------- |
| {$OS.USERNAME}          | Username to authenticate to OpenSearch with.                                                                       | CHANGE_ME |
| {$OS.PASSWORD}          | Password to authenticate to OpenSearch with.                                                                       | CHANGE_ME |
| {$OS.CLUSTER.HOSTNAME}  | Ideally a shared DNS name for the whole cluster, but a single node will also work. API requests will be sent here. | CHANGE_ME |
| {$OS.PORT}              | OpenSearch port for API requests.                                                                                  | `9200`    |
| {$OS.PROTOCOL}          | OpenSearch protocol being used (http/https).                                                                       | `https`   |
| {$OS.INDEX.PRIMARY_MAX} | Alert when an index's primary shard exceeds this size (in GB).                                                     | `60`      |

# Template links
There are no template links in this template.

# Discovery rules
This template has a rule for Node discovery. This is to allow for gathering Adaptive Selection ranking data to other nodes from the currrent node.

| Name           | Description                                                                    | Type         | Key and additional info |
| -------------- | ------------------------------------------------------------------------------ | ------------ | ----------------------- |
| Node Discovery | Discovery of other nodes for adaptive selection ranking from the current node. | `HTTP Agent` | opensearch.nodes        |

# Items collected

| Name                            | Description                                                                                                          | Type       | Key                                      |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------- | ---------- | ---------------------------------------- |
| Data Nodes Available            | Total nodes available to the cluster for data storage.                                                               | DEPENDENT  | opensearch.cluster.data_nodes            |
| Discovered Cluster Manager      | The cluster manager has been discovered.                                                                             | DEPENDENT  | opensearch.cluster.discovered_manager    |
| Timestamp                       | Timestamp as reported by the cluster. Pulled from Unix Epoch reported by cluster.                                    | DEPENDENT  | opensearch.cluster.epoch                 |
| Cluster Health                  | Overall health of cluster.                                                                                           | DEPENDENT  | opensearch.cluster.health                |
| Name                            | Name of the cluster, as reported by the cluster.                                                                     | DEPENDENT  | opensearch.cluster.name                  |
| Shards Percent Active           | Percentage of currently active and available shards in the cluster.                                                  | DEPENDENT  | opensearch.cluster.shards.active_percent |
| Shards Initializing             | Total number of currently initializing shards in the cluster.                                                        | DEPENDENT  | opensearch.cluster.shards.init           |
| Shards Primary                  | Total number of primary shards available and online in the cluster.                                                  | DEPENDENT  | opensearch.cluster.shards.pri            |
| Shards Relocating               | Total number of currently relocating shards in the cluster.                                                          | DEPENDENT  | opensearch.cluster.shards.relo           |
| Shards Secondary                | Total number of secondary shards available and online in the cluster (calculated by subtracting total from primary). | CALCULATED | opensearch.cluster.shards.second         |
| Shards                          | Total number of shards available and online in the cluster.                                                          | DEPENDENT  | opensearch.cluster.shards.total          |
| Shards Unassigned               | Total number of currently unassigned shards in the cluster.                                                          | DEPENDENT  | opensearch.cluster.shards.unassign       |
| Status                          | Cluster status (Green, Yellow, Red).                                                                                 | DEPENDENT  | opensearch.cluster.status                |
| Tasks Pending                   | Total number of currently pending tasks in the cluster.                                                              | DEPENDENT  | opensearch.cluster.tasks.pending         |
| Tasks Max Wait Time             | Wait time of longest pending tasks.                                                                                  | DEPENDENT  | opensearch.cluster.tasks.wait_time       |
| Nodes Available                 | Total nodes available to the cluster.                                                                                | DEPENDENT  | opensearch.cluster.total_nodes           |
| OpenSearch Cluster Cat Health   | Gather data from the API endpoint: _cat/health                                                                       | HTTP_AGENT | opensearchcluster.cat_health             |
| OpenSearch Cluster Cat Indices  | Gather data from the API endpoint: _cat/indices                                                                      | HTTP_AGENT | opensearchcluster.cat_indices            |
| OpenSearch Cluster Cat Nodes    | Gather data from the API endpoint: _cat/nodes                                                                        | HTTP_AGENT | opensearchcluster.cat_nodes              |
| OpenSearch Cluster Cat Tasks    | Gather data from the API endpoint: _cat/tasks                                                                        | HTTP_AGENT | opensearchcluster.cat_tasks              |
| Current Cluster Manager ID      | ID of the current cluster manager                                                                                    | DEPENDENT  | opensearchcluster.cluster_manager-id     |
| Current Cluster Manager Name    | Name of the current cluster manager                                                                                  | DEPENDENT  | opensearchcluster.cluster_manager-name   |
| Nodes OpenSearch Versions Match | Whether or not all OpenSearch versions are identical on all nodes in the cluster.                                    | DEPENDENT  | opensearchcluster.nodes.versionsmatch    |

## Item prototypes for Index discovery

| Name                                               | Description                                                                                                                                                    | Type       | Key                                                       |
| -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------------------------------------------------- |
| Index Docs Count - {#INDEX.NAME}                   | Number of available documents.                                                                                                                                 | DEPENDENT  | opensearchcluster.index.docs_count[{#INDEX.NAME}]         |
| Index Docs Deleted - {#INDEX.NAME}                 | Number of deleted documents.                                                                                                                                   | DEPENDENT  | opensearchcluster.index.docs_deleted[{#INDEX.NAME}]       |
| Index Health - {#INDEX.NAME}                       | Health of a specific index (Green, Yellow, Red).                                                                                                               | DEPENDENT  | opensearchcluster.index.health[{#INDEX.NAME}]             |
| Index Memory Utilization - {#INDEX.NAME}           | Total memory utilization for index between all nodes and all primary and replica shards.                                                                       | DEPENDENT  | opensearchcluster.index.memory[{#INDEX.NAME}]             |
| Index Memory Utilization (Primary) - {#INDEX.NAME} | Total memory utilization for index between all nodes and all primary shards (replicas not included).                                                           | DEPENDENT  | opensearchcluster.index.memory_primary[{#INDEX.NAME}]     |
| Index Memory Utilization (Replica) - {#INDEX.NAME} | Total memory utilization for index between all nodes and all replica shards (primaries not included).                                                          | CALCULATED | opensearchcluster.index.memory_replica[{#INDEX.NAME}]     |
| Index Search Throttled - {#INDEX.NAME}             | Whether or not an index is search throttled.                                                                                                                   | DEPENDENT  | opensearchcluster.index.search_throttled[{#INDEX.NAME}]   |
| Index Primary Shard Count - {#INDEX.NAME}          | Number of primary shards.                                                                                                                                      | DEPENDENT  | opensearchcluster.index.shards_primary[{#INDEX.NAME}]     |
| Index Replica Shard Count - {#INDEX.NAME}          | Number of replica shards.                                                                                                                                      | DEPENDENT  | opensearchcluster.index.shards_replica[{#INDEX.NAME}]     |
| Index Status - {#INDEX.NAME}                       | Open or Close: Open means data should be available and accessible; Close means data is not available and accessible, not using any RAM, but is stored on disk. | DEPENDENT  | opensearchcluster.index.status[{#INDEX.NAME}]             |
| Index Storage Size - {#INDEX.NAME}                 | Storage size of primary and replica shards.                                                                                                                    | DEPENDENT  | opensearchcluster.index.store_size[{#INDEX.NAME}]         |
| Index Storage Size (Primary) - {#INDEX.NAME}       | Storage size of only primary shards.                                                                                                                           | DEPENDENT  | opensearchcluster.index.store_size_primary[{#INDEX.NAME}] |
| Index Storage Size (Replica) - {#INDEX.NAME}       | Storage size of only replica shards (calculated by subtracting primary from total).                                                                            | CALCULATED | opensearchcluster.index.store_size_replica[{#INDEX.NAME}] |

# Triggers

| Name                                                    | Description                                                                        | Expression                                                                                                               | Severity | Dependencies                                                            | Additional Info   |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ | -------- | ----------------------------------------------------------------------- | ----------------- |
| OpenSearch Cluster data node count changed              | Count of available data nodes has changed for the cluster.                         | last(/OpenSearch-Cluster/opensearch.cluster.data_nodes,#2)<>last(/OpenSearch-Cluster/opensearch.cluster.data_nodes,#1)   | INFO     | OpenSearch Cluster node count changed                                   | Manual Close: YES |
| OpenSearch Cluster No Manager                           | Cluster does not have an assigned Cluster Manager node.                            | last(/OpenSearch-Cluster/opensearch.cluster.discovered_manager)="false"                                                  | HIGH     | None                                                                    | Manual Close: YES |
| OpenSearch Cluster Health: Red                          | Cluster currently has primary shards offline. Some data is unavailable.            | last(/OpenSearch-Cluster/opensearch.cluster.health)="red"                                                                | HIGH     | None                                                                    | Manual Close: YES |
| OpenSearch Cluster Health: Yellow                       | Cluster currently has replica shards offline. All primary shards are still online. | last(/OpenSearch-Cluster/opensearch.cluster.health)="yellow"                                                             | WARNING  | None                                                                    | Manual Close: YES |
| OpenSearch Cluster name has changed                     | OpenSearch cluster name was changed.                                               | last(/OpenSearch-Cluster/opensearch.cluster.name,#2)<>last(/OpenSearch-Cluster/opensearch.cluster.name,#1)               | INFO     | None                                                                    | Manual Close: YES |
| OpenSearch currently initializing shards: {ITEM.VALUE1} | Cluster is currently initializing shards.                                          | last(/OpenSearch-Cluster/opensearch.cluster.shards.init)<>0                                                              | INFO     | - OpenSearch Cluster Health: Red<br>- OpenSearch Cluster Health: Yellow | Manual Close: YES |
| OpenSearch currently unassigned shards: {ITEM.VALUE1}   | Cluster has unassigned (unavailable) shards.                                       | last(/OpenSearch-Cluster/opensearch.cluster.shards.init)<>0                                                              | AVERAGE  | - OpenSearch Cluster Health: Red<br>- OpenSearch Cluster Health: Yellow | Manual Close: YES |
| Nodes OpenSearch Version Mismatch                       | OpenSearch nodes version do not match.                                             | last(/OpenSearch-Cluster/opensearchcluster.nodes.versionsmatch)="false"                                                  | HIGH     | None                                                                    | None              |
| OpenSearch Cluster node count changed                   | Count of available nodes has changed for the cluster.                              | last(/OpenSearch-Cluster/opensearch.cluster.total_nodes,#2)<>last(/OpenSearch-Cluster/opensearch.cluster.total_nodes,#1) | INFO     | - OpenSearch Cluster Health: Red<br>- OpenSearch Cluster Health: Yellow | Manual Close: YES |

## Trigger prototypes for Index discovery

| Name                                                              | Description                                                                                                                                                 | Expression                                                                                                                                                                                                                                                          | Severity | Dependencies                                                            | Additional Info   |
| ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ----------------------------------------------------------------------- | ----------------- |
| Index Health is Red - {#INDEX.NAME}                               | Index health is red. Primary and secondary shards are offline. Index data is unavailable.                                                                   | last(/OpenSearch-Cluster/opensearchcluster.index.health[{#INDEX.NAME}],#1)="red" and last(/OpenSearch-Cluster/opensearchcluster.index.health[{#INDEX.NAME}],#2)="red" and last(/OpenSearch-Cluster/opensearchcluster.index.health[{#INDEX.NAME}],#3)="red"          | AVERAGE  | - OpenSearch Cluster Health: Red<br>- OpenSearch Cluster Health: Yellow | None              |
| Index Health is Yellow - {#INDEX.NAME}                            | Index health is yellow. All primary shards are still online, but secondary shards are unavailable. Data is still accessible.                                | last(/OpenSearch-Cluster/opensearchcluster.index.health[{#INDEX.NAME}],#1)="yellow" and last(/OpenSearch-Cluster/opensearchcluster.index.health[{#INDEX.NAME}],#2)="yellow" and last(/OpenSearch-Cluster/opensearchcluster.index.health[{#INDEX.NAME}],#3)="yellow" | WARNING  | - OpenSearch Cluster Health: Red<br>- OpenSearch Cluster Health: Yellow | None              |
| Index Search Throttled - {#INDEX.NAME}                            | Indices search operations have been throttled by the cluster manager. This can indicate a higher than usual request volume.                                 | last(/OpenSearch-Cluster/opensearchcluster.index.search_throttled[{#INDEX.NAME}])="true"                                                                                                                                                                            | WARNING  | None                                                                    | None              |
| OpenSearch Primary Shard Size Exceeded {$OS.INDEX.PRIMARY_MAX} GB | An index's primary shards have exceeded {$OS.INDEX.PRIMARY_MAX} GB. This can cause lots of performance, and write issues. This index should be rolled over. | last(/OpenSearch-Cluster/opensearchcluster.index.store_size_primary[{#INDEX.NAME}])>=({$OS.INDEX.PRIMARY_MAX} * 1073741824)                                                                                                                                         | AVERAGE  | None                                                                    | Manual Close: YES |

# Feedback
Please provide feedback on the template's [GitHub](https://github.com/landon-lengyel/Zabbix-OpenSearch)

# Known issues
