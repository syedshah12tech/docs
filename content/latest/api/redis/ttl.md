---
title: TTL
linkTitle: TTL
description: TTL
menu:
  latest:
    parent: api-redis
    weight: 2470
aliases:
  - api/redis/ttl
  - api/yedis/ttl
---

## Synopsis
<b>`TTL key`</b><br>
Returns the remaining time to live of a key that has a timeout, in seconds.

## Return Value
Returns TTL in seconds, encoded as integer response.

## Examples
```{.sh .copy .separator-dollar}
$ SET yugakey "YugaByte"
```
```sh
"OK"
```
```{.sh .copy .separator-dollar}
$ EXPIRE yugakey 10
```
```sh
(integer) 1
```
```{.sh .copy .separator-dollar}
$ TTL yugakey
```
```sh
(integer) 10
```

## See Also
[`set`](../set/), [`expire`](../expire/), [`expireat`](../expireat/), [`pttl`](../pttl/)