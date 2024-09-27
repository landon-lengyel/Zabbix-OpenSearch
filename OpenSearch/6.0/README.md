# Overview
Monitors individual OpenSearch nodes using their API's for various performance, and availability metrics.

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
 - `{$OS.HOSTNAME}`
 - `{$OS.NAME}` - Can be found in your nodes `opensearch.yml` file under `node.name`
 - `{$OS.ID}` - Can be found by running `GET _nodes` in OpenSearch Dashboards Dev tools. It is the random ID under the `"nodes": {}` section in the response.

# Macros used

| Name                                        | Description                                                           | Default   |
| ------------------------------------------- | --------------------------------------------------------------------- | --------- |
| {$OS.HOSTNAME}                              | Hostname of the OpenSearch cluster to make API requests to.           | CHANGE_ME |
| {$OS.NAME}                                  | Name of node (case sensitive).                                        | CHANGE_ME |
| {$OS.ID}                                    | OpenSearch automatically assigned ID for the node.                    | CHANGE_ME |
| {$OS.USERNAME}                              | Username to authenticate to OpenSearch with.                          | CHANGE_ME |
| {$OS.PASSWORD}                              | Password to authenticate to OpenSearch with.                          | CHANGE_ME |
| {$OS.FIELDDATA.EVICTIONS.MAX}               | Maximum FieldData eviction operations on node to throw alert at.      |           |
| {$OS.FILEDESCRIPTORS.MAX}                   | File Descriptor utilization percentage to throw alert at.             | `85`      |
| {$OS.GET.CURRENT.MAX}                       | Maximum Get operations on node to throw alert at.                     |           |
| {$OS.HEAP.MAX}                              | JVM Heap utilization percentage to throw alert at.                    | `85`      |
| {$OS.INDEXING.DELETE.MAX}                   | Maximum Deletion operations on node to throw alert at.                |           |
| {$OS.INDEXING.INDEX.MAX}                    | Maximum Indexing operations on node to throw alert at.                |           |
| {$OS.PORT}                                  | OpenSearch port that is being listened for API requests.              | `9200`    |
| {$OS.PROTOCOL}                              | OpenSearch protocol being used (http/https).                          | `https`   |
| {$OS.REQUESTCACHE.EVICTIONS.MAX}            | Maximum Request Cache Eviction operations on node to throw alert at.  |           |
| {$OS.REQUESTCACHE.MISSCOUNT.MAX}            | Maximum Request Cache Miss Count on node to throw alert at.           |           |
| {$OS.SEARCH.CONCURRENT_AVG_SLICE_COUNT.MAX} | Maximum Concurrent Average Slice counter on node to throw alert at.   |           |
| {$OS.SEARCH.CONCURRENT_QUERY.MAX}           | Maximum Search Concurrent Query operations on node to throw alert at. |           |
| {$OS.SEARCH.FETCH_CURRENT.MAX}              | Maximum Fetch Phase operations on node to throw alert at.             |           |
| {$OS.SEARCH.OPEN_CONTEXTS.MAX}              | Maximum Open Search Contexts on node to throw alert at.               |           |
| {$OS.SEARCH.QUERY.MAX}                      | Maximum Search Query operations on node to throw alert at.            |           |

# Template links
There are no template links in this template.

# Discovery rules
This template has a rule for Node discovery. This is to allow for gathering Adaptive Selection ranking data to other nodes from the currrent node.

| Name           | Description                                                                    | Type         | Key and additional info |
| -------------- | ------------------------------------------------------------------------------ | ------------ | ----------------------- |
| Node Discovery | Discovery of other nodes for adaptive selection ranking from the current node. | `HTTP Agent` | opensearch.nodes        |

# Items collected

| Name                      | Description                                                                                    | Type       | Key                                     |
| ------------------------- | ---------------------------------------------------------------------------------------------- | ---------- | --------------------------------------- |
| Disk Available            | Total disk available on the node. Not just available to OpenSearch, but the system as a whole. | DEPENDENT  | opensearch.allocation.disk.avail        |
| Disk Indices              | Size of indices currently stored on the nodes disk.                                            | DEPENDENT  | opensearch.allocation.disk.indices      |
| Disk Percentage Used      | Percentage of disk storage in use on node. Not just by OpenSearch, but the system as a whole.  | DEPENDENT  | opensearch.allocation.disk.percent      |
| Disk Used                 | Total disk used on server, not just by OpenSearch, but the system as a whole.                  | DEPENDENT  | opensearch.allocation.disk.used         |
| Shard Count               | Shard count currently stored on the node.                                                      | DEPENDENT  | opensearch.allocation.shards            |
| OpenSearch Cat Allocation | Gather data from the API endpoint: _cat/allocation                                             | HTTP_AGENT | opensearch.cat_allocation               |
| OpenSearch Cat Nodes      | Gather data from the API endpoint: _cat/nodes                                                  | HTTP_AGENT | opensearch.cat_nodes                    |
| Package Build Number      | OpenSearch distribution package's build number.                                                | DEPENDENT  | opensearch.node.build                   |
| Cluster Manager           | True if the node is the cluster manager, False if not.                                         | DEPENDENT  | opensearch.node.cluster_manager         |
| Completion Size           | Size of completion                                                                             | DEPENDENT  | opensearch.node.completion.size         |
| CPU                       | Recent CPU utilization.                                                                        | DEPENDENT  | opensearch.node.cpu                     |
| Disk Available            | Total disk space available to the system.                                                      | DEPENDENT  | opensearch.node.disk.avail              |
| Disk Used                 | Total disk space used in OpenSearch.                                                           | DEPENDENT  | opensearch.node.disk.used               |
| Disk Used Percent         | Total disk space used in OpenSearch (as a percentage of total)                                 | DEPENDENT  | opensearch.node.disk.used_percent       |
| FieldData Evictions       | FieldData Evictions                                                                            | DEPENDENT  | opensearch.node.fielddata.evictions     |
| FieldData Memory Size     | Used FieldData cache                                                                           | DEPENDENT  | opensearch.node.fielddata.memory_size   |
| File Descriptors Current  | Currently used file descriptors.                                                               | DEPENDENT  | opensearch.node.file_desc.current       |
| File Descriptors Max      | Maximum possible file descriptors.                                                             | DEPENDENT  | opensearch.node.file_desc.max           |
| File Descriptors Percent  | Currently used file descriptors expressed as a percentage of maximum file descriptors.         | DEPENDENT  | opensearch.node.file_desc.percent       |
| Flush Changes             | Zabbix calculated difference of last value and current value.                                  | DEPENDENT  | opensearch.node.flush.changes           |
| Flush Total               | Total number of flushes.                                                                       | DEPENDENT  | opensearch.node.flush.total             |
| Flush Total Time          | Time spent in flush                                                                            | DEPENDENT  | opensearch.node.flush.total_time        |
| Get Changes               | Zabbix calculated difference of last value and current value.                                  | DEPENDENT  | opensearch.node.get.changes             |
| Get Current               | Current number of get operations.                                                              | DEPENDENT  | opensearch.node.get.current             |
| Get Exists Changes        | Zabbix calculated difference of last value and current value.                                  | DEPENDENT  | opensearch.node.get.exists_changes      |
| Get Exists Time           | Time spent in successful get operations.                                                       | DEPENDENT  | opensearch.node.get.exists_time         |
| Get Exists Total          | Total number of successful get operations.                                                     | DEPENDENT  | opensearch.node.get.exists_total        |
| Get Missing Changes       | Zabbix calculated difference of last value and current value.                                  | DEPENDENT  | opensearch.node.get.missing_changes     |
| Get Missing Time          | Time spent in failed get operations.                                                           | DEPENDENT  | opensearch.node.get.missing_time        |
| Get Missing Total         | Total number of failed get operations.                                                         | DEPENDENT  | opensearch.node.get.missing_total       |
| Get Time                  | Time spent in get                                                                              | DEPENDENT  | opensearch.node.get.time                |
| Get Total                 | Total number of get operations.                                                                | DEPENDENT  | opensearch.node.get.total               |
| Heap Current              | Currently used Java heap. These are Java objects in memory.                                    | DEPENDENT  | opensearch.node.heap.current            |
| Heap Max                  | Maximum configured Java heap. These are Java objects in memory.                                | DEPENDENT  | opensearch.node.heap.max                |
| Heap Percent              | Currently used Java heap. These are Java objects in memory. (Expressed as a percent of max)    | DEPENDENT  | opensearch.node.heap.percent            |
| HTTP Address              | HTTP URL the node is listening on.                                                             | DEPENDENT  | opensearch.node.http_address            |
| ID                        | Cluster assigned random ID                                                                     | DEPENDENT  | opensearch.node.id                      |
| Delete Current            | Current number of deletion operations on indices.                                              | DEPENDENT  | opensearch.node.indexing.delete_current |
| Delete Time               | Time spent in index deletion operations.                                                       | DEPENDENT  | opensearch.node.indexing.delete_time    |
| Indexing Current          | Current number of indexing operations.                                                         | DEPENDENT  | opensearch.node.indexing.index_current  |
| Indexing Total Failed     | Total number of failed indexing operations.                                                    | DEPENDENT  | opensearch.node.indexing.index_failed   |
| Indexing Time             | Time spent in indexing operations.                                                             | DEPENDENT  | opensearch.node.indexing.index_time     |

# Triggers

| Name                                         | Description                                                                            | Expression                                                                              | Severity | Dependencies |
| -------------------------------------------- | -------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | -------- | ------------ |
| Node is not responding                       | Attempted to reach the API on node, but hasn't been responding for at least 3 minutes. | nodata(/OpenSearch/opensearch.cat_nodes,3m)=1                                           | HIGH     |              |
| OpenSearch FieldData Evictions High          | OpenSearch node's FieldData Eviction operations are higher than configured threshold.  | last(/OpenSearch/opensearch.node.fielddata.evictions,#1)>={$OS.FIELDDATA.EVICTIONS.MAX} | WARNING  |              |
| OpenSearch File Descriptors Availability Low | OpenSearch node's available file descriptors have crossed below the defined threshold. | last(/OpenSearch/opensearch.node.file_desc.percent,#1)>={$OS.FILEDESCRIPTORS.MAX}       | WARNING  |              |
| OpenSearch Get Operations High               | OpenSearch node's get operations are higher than configured threshold.                 | last(/OpenSearch/opensearch.node.get.current,#1)>={$OS.GET.CURRENT.MAX}                 | WARNING  |              |
| OpenSearch JVM Heap Utilization High         | OpenSearch node's JVM heap utilization is higher than configured threshold.            | last(/OpenSearch/opensearch.node.heap.percent,#1)>={$OS.HEAP.MAX}                       | WARNING  |              |
| OpenSearch Delete Operations High            | OpenSearch node's deletion operations are higher than configured threshold.            | last(/OpenSearch/opensearch.node.indexing.delete_current,#1)>={$OS.INDEXING.DELETE.MAX} | WARNING  |              |
| OpenSearch Indexing Operations High          | OpenSearch node's indexing operations are higher than configured threshold.            | last(/OpenSearch/opensearch.node.indexing.index_current,#1)>={$OS.INDEXING.INDEX.MAX}   | WARNING  |              |

# Feedback

# Demo

# Known issues

# References
