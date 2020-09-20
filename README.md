# Task

1. Provide a solution code and setup CI/CD pipeline that will automatically:

    1.1. Setup a production-like three-Node MongoDB Cluster in Kubernetes. Assume Kubernetes cluster already in place.
    1.2. Create a unique mongo-collection and a user with write permissions to that collection  
    1.3. The output should include a collection with user credentials  
    1.4. Validate deployment.  
    1.5. Expose an endpoint for Prometheus metrics.  

2. Pick a set of metrics to ensure reliable functionality of the cluster

Notes:  *production-like* is not meant in sense of performance, but in sense of maintenance.

---


## How to run
```
./new-mongodb.sh
```

you can add running this script in your pipeline.
we suppose we run this script on a runner with an access to k8s cluster and on the runner we have pre-installed kubectl and helm

## Metric  to ensure reliable functionality of the cluster
```bash
# HELP mongodb_up Whether MongoDB is up.
# TYPE mongodb_up gauge
mongodb_up 1

# HELP process_cpu_seconds_total Total user and system CPU time spent in seconds.
# TYPE process_cpu_seconds_total counter
process_cpu_seconds_total 1.91

# HELP process_max_fds Maximum number of open file descriptors.
# TYPE process_max_fds gauge
process_max_fds 1.048576e+06
# HELP process_open_fds Number of open file descriptors.
# TYPE process_open_fds gauge
process_open_fds 11

# HELP process_resident_memory_bytes Resident memory size in bytes.
# TYPE process_resident_memory_bytes gauge
process_resident_memory_bytes 2.492416e+07

# HELP mongodb_mongod_replset_member_health This field conveys if the member is up (1) or down (0).
# TYPE mongodb_mongod_replset_member_health gauge
mongodb_mongod_replset_member_health{name="otus-mongodb-0.otus-mongodb-headless.mongodb.svc.cluster.local:27017",set="otustest",state="PRIMARY"} 1
mongodb_mongod_replset_member_health{name="otus-mongodb-1.otus-mongodb-headless.mongodb.svc.cluster.local:27017",set="otustest",state="SECONDARY"} 1
mongodb_mongod_replset_member_health{name="otus-mongodb-2.otus-mongodb-headless.mongodb.svc.cluster.local:27017",set="otustest",state="SECONDARY"} 1

# HELP mongodb_mongod_replset_member_operational_lag The operationl lag - or staleness of the oplog timestamp - for this member.
# TYPE mongodb_mongod_replset_member_operational_lag gauge
mongodb_mongod_replset_member_operational_lag{name="otus-mongodb-1.otus-mongodb-headless.mongodb.svc.cluster.local:27017",set="otustest",state="SECONDARY"} 1
mongodb_mongod_replset_member_operational_lag{name="otus-mongodb-2.otus-mongodb-headless.mongodb.svc.cluster.local:27017",set="otustest",state="SECONDARY"} 1

# HELP mongodb_mongod_replset_member_replication_lag The replication lag that this member has with the primary.
# TYPE mongodb_mongod_replset_member_replication_lag gauge
mongodb_mongod_replset_member_replication_lag{name="otus-mongodb-1.otus-mongodb-headless.mongodb.svc.cluster.local:27017",set="otustest",state="SECONDARY"} 0
mongodb_mongod_replset_member_replication_lag{name="otus-mongodb-2.otus-mongodb-headless.mongodb.svc.cluster.local:27017",set="otustest",state="SECONDARY"} 0



# HELP mongodb_mongod_replset_member_last_heartbeat The lastHeartbeat value provides an ISODate formatted date and time of the transmission time of last heartbeat received from this member
# TYPE mongodb_mongod_replset_member_last_heartbeat gauge
mongodb_mongod_replset_member_last_heartbeat{name="otus-mongodb-0.otus-mongodb-headless.mongodb.svc.cluster.local:27017",set="otustest",state="PRIMARY"} 1.599730925e+09
mongodb_mongod_replset_member_last_heartbeat{name="otus-mongodb-1.otus-mongodb-headless.mongodb.svc.cluster.local:27017",set="otustest",state="SECONDARY"} 1.599730925e+09
# HELP mongodb_mongod_replset_member_last_heartbeat_recv The lastHeartbeatRecv value provides an ISODate formatted date and time that the last heartbeat was received from this member
# TYPE mongodb_mongod_replset_member_last_heartbeat_recv gauge
mongodb_mongod_replset_member_last_heartbeat_recv{name="otus-mongodb-0.otus-mongodb-headless.mongodb.svc.cluster.local:27017",set="otustest",state="PRIMARY"} 1.599730925e+09
mongodb_mongod_replset_member_last_heartbeat_recv{name="otus-mongodb-1.otus-mongodb-headless.mongodb.svc.cluster.local:27017",set="otustest",state="SECONDARY"} 1.599730925e+09

# HELP mongodb_mongod_op_latencies_latency_total op latencies statistics in microseconds of mongod
# TYPE mongodb_mongod_op_latencies_latency_total gauge
mongodb_mongod_op_latencies_latency_total{type="command"} 362040
mongodb_mongod_op_latencies_latency_total{type="read"} 36615
mongodb_mongod_op_latencies_latency_total{type="write"} 0
# HELP mongodb_mongod_op_latencies_ops_total op latencies ops total statistics of mongod
# TYPE mongodb_mongod_op_latencies_ops_total gauge
mongodb_mongod_op_latencies_ops_total{type="command"} 1527
mongodb_mongod_op_latencies_ops_total{type="read"} 292
mongodb_mongod_op_latencies_ops_total{type="write"} 0

# HELP mongodb_mongod_wiredtiger_log_operations_total The total number of WiredTiger log operations
# TYPE mongodb_mongod_wiredtiger_log_operations_total counter
mongodb_mongod_wiredtiger_log_operations_total{type="flush"} 3605
mongodb_mongod_wiredtiger_log_operations_total{type="read"} 0
mongodb_mongod_wiredtiger_log_operations_total{type="scan"} 4
mongodb_mongod_wiredtiger_log_operations_total{type="scan_double"} 3
mongodb_mongod_wiredtiger_log_operations_total{type="sync"} 49
mongodb_mongod_wiredtiger_log_operations_total{type="sync_dir"} 1

# HELP mongodb_op_counters_repl_total The opcountersRepl data structure, similar to the opcounters data structure, provides an overview of database replication operations by type and makes it possible to analyze the load on the replica in more granular manner. These values only appear when the current host has replication enabled
# TYPE mongodb_op_counters_repl_total counter
mongodb_op_counters_repl_total{type="command"} 0
mongodb_op_counters_repl_total{type="delete"} 0
mongodb_op_counters_repl_total{type="getmore"} 0
mongodb_op_counters_repl_total{type="insert"} 3
mongodb_op_counters_repl_total{type="query"} 0
mongodb_op_counters_repl_total{type="update"} 0
# HELP mongodb_op_counters_total The opcounters data structure provides an overview of database operations by type and makes it possible to analyze the load on the database in more granular manner. These numbers will grow over time and in response to database use. Analyze these values over time to track database utilization
# TYPE mongodb_op_counters_total counter
mongodb_op_counters_total{type="command"} 1545
mongodb_op_counters_total{type="delete"} 0
mongodb_op_counters_total{type="getmore"} 0
mongodb_op_counters_total{type="insert"} 0
mongodb_op_counters_total{type="query"} 297
mongodb_op_counters_total{type="update"} 0

```


