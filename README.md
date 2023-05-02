# DS201 Exercise 15

## Write Path

**In this hands-on lab, you will:**
*	Watch the commit log as data is written to the database
*	Use nodetool to view statistics for a CQL table
*	Force data that has been written in memory to flush to disk
*	Look for commit log segments being replayed when starting a node


Consistency is the tricky challenge for distributed systems. As distributed systems trade-off consistency for performance, some of the nodes in a cluster may become inconsistent. When Cassandra notices these inconsistencies, it takes steps to resolve them. This resolution is the role of Write Path.

## Start the hands-on lab:

[![Open in KataPod](https://github.com/DataStax-Academy/katapod-shared-assets/blob/main/images/open-in-katapod.png)](https://gitpod.io/##https://github.com/DataStax-Academy/ds201-lab15/)