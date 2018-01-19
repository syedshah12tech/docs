---
title: Cannot Connect to Redis API Service
weight: 824
---

## 1. Are YugaByte processes running?

First, ensure that the expected YugaByte DB processes on the current node.
At a minimum, the tserver process needs to be running to be able to connect to this node with a Redis client or application.
Additionally, depending on the setup, you might expect a master process to also be running on this node.
Follow the instructions on the [Check Processes](/troubleshoot/nodes/check-processes/) page.

## 2. Is the YugaByte DB Redis server running?

If the tserver process is running, make sure the YugaByte Redis server is enabled and listening on the Redis port (default `6379`).

```
$ lsof -i :6379
COMMAND     PID   USER   FD   TYPE     DEVICE SIZE/OFF NODE NAME
yb-tserve 81590 centos   92u  IPv4 0xdeadbeef      0t0  TCP localhost:6379 (LISTEN)
```

Note: when running a local cluster with `yb-ctl` you should see all the nodes here with different IPs. For instance:
```
$ lsof -i :6379
COMMAND     PID   USER   FD   TYPE     DEVICE SIZE/OFF NODE NAME
yb-tserve 81590 centos   92u  IPv4 0xdeadbeef      0t0  TCP localhost:6379 (LISTEN)
yb-tserve 81593 centos   92u  IPv4 0xdeadbeef      0t0  TCP 127.0.0.2:6379 (LISTEN)
yb-tserve 81596 centos   92u  IPv4 0xdeadbeef      0t0  TCP 127.0.0.3:6379 (LISTEN)
```
If there is another process using this port you might need to stop that and restart the tserver process.
Otherwise, if no process is listening but the tserver is running, check the value of the `--redis_proxy_bind_address` flag passed to the 
tserver process.

## 3. Can the Redis CLI can connect locally?

Use `redis-cli` to connect to the local node.
You may need to install `redis-cli`, otherwise you can find it in the YugaByte bin directory). 
```
./redis-cli 127.0.0.1
```

If `redis-cli` can connect, the issue is likely a network issue with the original client not being able to access this node where YugaByte DB is running. See also [Cannot access Master or TServer Endpoints](#cannot-access-master-or-tserver-endpoints) below.
Otherwise, you might need to run `./yb-admin --master_addresses <master-ip-addresses> setup_redis_table"`. You can find the `yb-admin` tool in the YugaByte `bin` directory.