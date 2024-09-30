# Introduction

These two templates should assist with monitoring OpenSearch clusters with Zabbix. 
I recommend using both templates, as they monitor different things.

The **OpenSearch** template monitors individual nodes. Triggers are focused around issues with the specific node such as it being offline, or performance issues on the node.
The **OpenSearch Cluster** template monitors the cluster as a whole. It discovers all indices, and has triggers if there are issues with those indices, or around the whole cluster's status.

See the each templates `README.md` file for assistance on configuring them.

This template only uses [HTTP Agent](https://www.zabbix.com/documentation/current/en/manual/config/items/itemtypes/http) requests and the API calls will come from your Zabbix Server or proxy to your OpenSearch node. Ensure all relevant firewall rules are in place to allow that traffic.

# Feedback

Please reach out with any issues you discover, or things you'd like to see added through the repo's Issues.
