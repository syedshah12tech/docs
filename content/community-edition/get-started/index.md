---
date: 2016-03-09T00:11:02+01:00
title: Community Edition - Start a local cluster
weight: 15
---

 The easiest way to get started with the YugaByte Community Edition is to start a multi-node YugaByte **local cluster** on your laptop or desktop.

{{< note title="Note" >}}
Running local clusters is not recommended for production environments. You can either deploy the Community Edition manually on a set of instances or use the Enterprise Edition that automates all day-to-day operations including cluster administration across all major public clouds as well as on-premises datacenters.
{{< /note >}}

## Prerequisites

Operation systems supported for local clusters are
- Centos 7 or higher

## Download and install

```sh
$ wget 
$ mkdir yugabyte
$ tar xvfz yugabyte.a0a845f2ba6ff2d4436c3b0525ddae28075fbbc4.0.0.19.25.tar.gz -C yugabyte
$ cd yugabyte
$ ./bin/configure.sh 
```

## Start the cluster

```sh
$ ./bin/yugabyte.py --rf 3 create
```

Use the **-h** option to see all the commands the yugabyte.py script supports

```sh
$ ./bin/yugabyte.py -h
usage: yugabyte.py [-h] [--binary_dir BINARY_DIR]
                   [--replication_factor REPLICATION_FACTOR]
                   {create,destroy,status,add_node,remove_node} ...

positional arguments:
  {create,destroy,status,add_node,remove_node}
    create              Create a new cluster
    destroy             Destroy the current cluster
    status              Get info on the current cluster processes
    add_node            Add a new tserver to the current cluster
    remove_node         Remove a tserver from the current cluster

optional arguments:
  -h, --help            show this help message and exit
  --binary_dir BINARY_DIR
                        Specify a custom directory in which to find the
                        yugabyte binaries.
  --replication_factor REPLICATION_FACTOR, --rf REPLICATION_FACTOR
                        Replication factor for the cluster as well as default
                        number of masters.
```


## Connect with cqlsh or redis-cli

### cqlsh

[**cqlsh**](http://cassandra.apache.org/doc/latest/tools/cqlsh.html) is a command line shell for interacting with Apache Cassandra through [CQL (the Cassandra Query Language)](http://cassandra.apache.org/doc/latest/cql/index.html). It is shipped with every Cassandra package, and can be found in the bin/ directory. cqlsh utilizes the Python CQL driver, and connects to the single node specified on the command line.

- Download and run cqlsh

```sh
$ docker run -it --rm quay.io/yugabyte/cqlsh localhost:9043
```

- Output will look similar to the following

```sh
Connected to local cluster at localhost:9043.
[cqlsh 5.0.1 | Cassandra 3.9-SNAPSHOT | CQL spec 3.4.2 | Native protocol v4]
Use HELP for help.
cqlsh> 
```

- Run a cql command

```sh
cqlsh> describe keyspaces;

system_schema  system  default_keyspace

cqlsh> 

```

### redis-cli

## Run a sample app

- Verify that Java is installed on your localhost.

```sh
$ java -version
```

### Cassandra sample app

- Run the Cassandra time-series sample app using the executable jar

```sh
$ java -jar ./java/yb-sample-apps.jar --workload CassandraTimeseries --nodes localhost:9043
```

You will see the following output.

```sh
09:37 $ java -jar /tmp/yb-sample-apps.jar --workload CassandraTimeseries --nodes localhost:9043
2017-06-29 09:37:07,601 [INFO|com.yugabyte.sample.common.CmdLineOpts|CmdLineOpts] Using a randomly generated UUID : b406bcaa-efd6-4cc4-b13d-2826098bef28
2017-06-29 09:37:07,610 [INFO|com.yugabyte.sample.common.CmdLineOpts|CmdLineOpts] App: CassandraTimeseries
2017-06-29 09:37:07,610 [INFO|com.yugabyte.sample.common.CmdLineOpts|CmdLineOpts] Adding node: localhost:9042
2017-06-29 09:37:07,611 [INFO|com.yugabyte.sample.common.CmdLineOpts|CmdLineOpts] Num reader threads: 1, num writer threads: 16
2017-06-29 09:37:07,614 [INFO|com.yugabyte.sample.common.CmdLineOpts|CmdLineOpts] Num unique keys to insert: 0
2017-06-29 09:37:07,614 [INFO|com.yugabyte.sample.common.CmdLineOpts|CmdLineOpts] Num keys to update: -1
2017-06-29 09:37:07,614 [INFO|com.yugabyte.sample.common.CmdLineOpts|CmdLineOpts] Num keys to read: -1
2017-06-29 09:37:07,614 [INFO|com.yugabyte.sample.common.CmdLineOpts|CmdLineOpts] Value size: 100
2017-06-29 09:37:07,614 [INFO|com.yugabyte.sample.common.CmdLineOpts|CmdLineOpts] Table TTL (secs): 86400
2017-06-29 09:37:09,374 [INFO|com.yugabyte.sample.apps.AppBase|AppBase] Ignoring exception dropping table: SQL error (yb/sql/ptree/process_context.cc:41): SQL Error (1.11): Table Not Found - Not found (yb/common/wire_protocol.cc:120): The table does not exist: table_name: "ts_metrics_raw"
namespace {
  name: "ybdemo_keyspace"
}

	ts_metrics_raw;
	^^^^^^^^^^^^^^
 (error -301)
2017-06-29 09:37:11,081 [INFO|com.yugabyte.sample.apps.AppBase|AppBase] Created a Cassandra table using query: [CREATE TABLE IF NOT EXISTS ts_metrics_raw (  user_id varchar, metric_id varchar, node_id varchar, ts timestamp, value varchar, primary key ((user_id, metric_id), node_id, ts)) WITH default_time_to_live = 86400;]
2017-06-29 09:37:12,684 [INFO|com.yugabyte.sample.common.metrics.MetricsTracker|MetricsTracker] Read: 61.20 ops/sec (4.42 ms/op), 306 total ops  |  Write: 587.90 ops/sec (6.27 ms/op), 2941 total ops  |  Uptime: 5069 ms | Verification: ON | 
2017-06-29 09:37:17,688 [INFO|com.yugabyte.sample.common.metrics.MetricsTracker|MetricsTracker] Read: 318.02 ops/sec (3.14 ms/op), 1898 total ops  |  Write: 2517.02 ops/sec (3.89 ms/op), 15536 total ops  |  Uptime: 10073 ms | Verification: ON | 
2017-06-29 09:37:22,688 [INFO|com.yugabyte.sample.common.metrics.MetricsTracker|MetricsTracker] Read: 242.98 ops/sec (4.11 ms/op), 3113 total ops  |  Write: 3128.72 ops/sec (4.53 ms/op), 31181 total ops  |  Uptime: 15073 ms | Verification: ON | 
2017-06-29 09:37:27,690 [INFO|com.yugabyte.sample.common.metrics.MetricsTracker|MetricsTracker] Read: 222.93 ops/sec (4.48 ms/op), 4228 total ops  |  Write: 3294.00 ops/sec (4.54 ms/op), 47656 total ops  |  Uptime: 20075 ms | Verification: ON | 
 
```

As you can see above, the Cassandra time-series sample app first creates a keyspace `ybdemo_keyspace` and a table `ts_metrics_raw`. It then starts multiple writer and reader threads to generate the load.

- Verify using cqlsh

```sh
cqlsh> use ybdemo_keyspace;
cqlsh:ybdemo_keyspace> describe tables;

ts_metrics_raw

cqlsh:ybdemo_keyspace> describe ts_metrics_raw

CREATE TABLE ybdemo_keyspace.ts_metrics_raw (
    user_id text,
    metric_id text,
    node_id text,
    ts timestamp,
    value text,
    PRIMARY KEY ((user_id, metric_id), node_id, ts)
) WITH CLUSTERING ORDER BY (node_id ASC, ts ASC);

cqlsh:ybdemo_keyspace> select * from ts_metrics_raw limit 10;

 user_id                                 | metric_id                 | node_id    | ts                              | value
-----------------------------------------+---------------------------+------------+---------------------------------+--------------------------
 48-b406bcaa-efd6-4cc4-b13d-2826098bef28 | metric-00007.yugabyte.com | node-00000 | 2017-06-29 16:37:12.000000+0000 | 1498754232000[B@3810f07d
 48-b406bcaa-efd6-4cc4-b13d-2826098bef28 | metric-00007.yugabyte.com | node-00000 | 2017-06-29 16:37:13.000000+0000 | 1498754233000[B@4507cba7
 48-b406bcaa-efd6-4cc4-b13d-2826098bef28 | metric-00007.yugabyte.com | node-00000 | 2017-06-29 16:37:14.000000+0000 | 1498754234000[B@23a7416b
 48-b406bcaa-efd6-4cc4-b13d-2826098bef28 | metric-00007.yugabyte.com | node-00000 | 2017-06-29 16:37:15.000000+0000 | 1498754235000[B@7eb9c5f4
 48-b406bcaa-efd6-4cc4-b13d-2826098bef28 | metric-00007.yugabyte.com | node-00000 | 2017-06-29 16:37:16.000000+0000 | 1498754236000[B@4957efc7
 48-b406bcaa-efd6-4cc4-b13d-2826098bef28 | metric-00007.yugabyte.com | node-00000 | 2017-06-29 16:37:17.000000+0000 | 1498754237000[B@2a4c4735
 48-b406bcaa-efd6-4cc4-b13d-2826098bef28 | metric-00007.yugabyte.com | node-00000 | 2017-06-29 16:37:18.000000+0000 | 1498754238000[B@63638401
 48-b406bcaa-efd6-4cc4-b13d-2826098bef28 | metric-00007.yugabyte.com | node-00000 | 2017-06-29 16:37:19.000000+0000 | 1498754239000[B@419a913d
 48-b406bcaa-efd6-4cc4-b13d-2826098bef28 | metric-00007.yugabyte.com | node-00000 | 2017-06-29 16:37:20.000000+0000 | 1498754240000[B@25b246f2
 48-b406bcaa-efd6-4cc4-b13d-2826098bef28 | metric-00007.yugabyte.com | node-00000 | 2017-06-29 16:37:21.000000+0000 | 1498754241000[B@38d35bcc

(10 rows)
cqlsh:ybdemo_keyspace>

```

### Redis sample app

- Run the Redis key-value sample app using the executable jar

```sh
$ java -jar ./java/yb-sample-apps.jar --workload RedisKeyValue --nodes localhost:6379
```

## Add a node

## Remove a node

## Terminate the cluster


