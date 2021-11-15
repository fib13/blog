+++
title = "Elasticsearch 问题排查"
description = ""
weight = 1
+++

问题列表如下：

- [集群中包含未分配的副本导致集群变为 Red 状态](#集群中包含未分配的副本导致集群变为-red-状态) 

## 集群中包含未分配的副本导致集群变为 Red 状态

### 查看集群状态信息

首先确定原因。

#### 查看整体健康信息

```bash
$ curl -u 'elastic:password' 'http://127.0.0.1:9200/_cluster/health?pretty'
{
  "cluster_name" : "elasticsearch",
  "status" : "red",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 69,
  "active_shards" : 69,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 1,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 98.57142857142858
}
```

**主要关注其中的 `unassigned_shards` 指标**，其代表已经在集群状态中存在的分片，但是实际在集群里又找不着。通常未分配分片的来源是未分配的副本。比如，一个有 5 分片和 1 副本的索引，在单节点集群上，就会有 5 个未分配副本分片。如果你的集群是 red 状态，也会长期保有未分配分片（因为缺少主分片）。其他指标解释：

- `initializing_shards` 是刚刚创建的分片的个数。比如，当你刚创建第一个索引，分片都会短暂的处于 initializing 状态。这通常会是一个临时事件，分片不应该长期停留在 initializing 状态。你还可能在节点刚重启的时候看到 initializing 分片：当分片从磁盘上加载后，它们会从 initializing 状态开始。

- `number_of_nodes` 和 `number_of_data_nodes`：可由字面意思理解。
- `active_primary_shards`：指出你集群中的主分片数量。这是涵盖了所有索引的汇总值。
- `active_shards`：所有索引的所有分片的汇总值，即包括副本分片。
- `relocating_shards`：显示当前正在从一个节点迁往其他节点的分片的数量。通常来说应该是 0，不过在 Elasticsearch 发现集群不太均衡时，该值会上涨。比如说：添加了一个新节点，或者下线了一个节点。
- `active_shards_percent_as_number`：活跃分片数占总分片数比例。 

#### 查看每个索引的健康信息

我们还可以通过 `level` 参数查看更详细的信息：

```bash
curl -u elastic:password 'http://127.0.0.1:9200/_cluster/health?pretty=true&level=indices'
{
  "cluster_name" : "elasticsearch",
  "status" : "red",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 69,
  "active_shards" : 69,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 2,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 97.1830985915493,
  "indices" : {
...
    "kubernetes-2021.08.13" : {
      "status" : "green",
      "number_of_shards" : 1,
      "number_of_replicas" : 0,
      "active_primary_shards" : 1,
      "active_shards" : 1,
      "relocating_shards" : 0,
      "initializing_shards" : 0,
      "unassigned_shards" : 1
    },
...
  }
```

添加 `level=indices` 参数会在集群健康信息里添加一个索引清单，以及有关每个索引的细节（状态、分片数、未分配分片数等等），一旦我们询问要索引的输出，哪个索引有问题立马就很清楚了：`kubernetes-2021.08.13`。

#### 查看每个索引的每个分片的健康信息

我们可以更细致的输出索引的每个分片的健康信息，通过添加参数 `level=shards` 获取。

```bash
curl -u 'elastic:password' 'http://127.0.0.1:9200/_cluster/health?pretty=true&level=shards'
```

通过添加 `level=indices` 参数可以列出每个索引里每个分片的状态和位置。这个输出有时候很有用，但是由于太过详细会比较难用。

#### 查看未分配分片的索引及原因

```bash
curl -u 'elastic:password' 'http://127.0.0.1:9200/_cat/shards?v&h=index,shard,prirep,state,unassigned.reason'
index                    shard prirep state      unassigned.reason
.apm-custom-link         0     p      STARTED    
... 
kubernetes-2021.10.10    0     p      STARTED    
kubernetes-2021.11.03    0     p      UNASSIGNED CLUSTER_RECOVERED
kubernetes-2021.08.19    0     p      STARTED    
...
```

`unassigned.reason` 值说明：

- `INDEX_CREATED`:  由于创建索引的 API 导致未分配。
- `CLUSTER_RECOVERED`:  由于完全集群恢复导致未分配。
- `INDEX_REOPENED`:  由于打开 open 或关闭 close 一个索引导致未分配。
- `DANGLING_INDEX_IMPORTED`:  由于导入 dangling 索引的结果导致未分配。
- `NEW_INDEX_RESTORED`:  由于恢复到新索引导致未分配。
- `EXISTING_INDEX_RESTORED`:  由于恢复到已关闭的索引导致未分配。
- `REPLICA_ADDED`:  由于显式添加副本分片导致未分配。
- `ALLOCATION_FAILED`:  由于分片分配失败导致未分配。
- `NODE_LEFT`:  由于承载该分片的节点离开集群导致未分配。
- `REINITIALIZED`:  由于当分片从开始移动到初始化时导致未分配（例如，使用Shadow 副本分片）。
- `REROUTE_CANCELLED`:  作为显式取消重新路由命令的结果取消分配。
- `REALLOCATED_REPLICA`:  确定更好的副本位置被标定使用，导致现有的副本分配被取消，出现未分配。


### 解决方法

由两种方法可解决，任选其中一种即可。

#### 手动分配

```bash
curl -u 'elastic:password' -H "Content-Type: application/json" 'http://127.0.0.1:9200/_cluster/reroute' -d '{
    "commands": [{
        "allocate": {
            "index": "<索引名称>",
            "shard": <分片编号>,
            "node": "<节点 ID>",
            "allow_primary": true
        }
    }]
}'
```

批处理脚本：

```bash
#!/bin/bash
array=( node1 node2 node3 )
node_counter=0
length=${#array[@]}
IFS=$'\n'
for line in $(curl -s 'http://127.0.0.1:9200/_cat/shards'|  fgrep UNASSIGNED); do
    INDEX=$(echo $line | (awk '{print $1}'))
    SHARD=$(echo $line | (awk '{print $2}'))
    NODE=${array[$node_counter]}
    echo $NODE
    curl -XPOST 'http://127.0.0.1:9200/_cluster/reroute' -d '{
        "commands": [
        {
            "allocate": {
                "index": "'$INDEX'",
                "shard": '$SHARD',
                "node": "'$NODE'",
                "allow_primary": true
            }
        }
        ]
    }'
    node_counter=$(((node_counter)%length +1))
done
```

#### 重新建立副本

如果在上面的命令执行输出结果中，假如所有的 `primary shards` 都是正常的，所有 `replica shards` 有问题，有一种快速恢复的方法，就是强制删除掉 `replica shards`，让 Elasticsearch 自主重新生成。

首先先将出问题的索引的副本为 0：

```bash
curl -u 'elastic:password' -H "Content-Type: application/json" -X PUT 'http://127.0.0.1:9200/<索引名称>/_settings' -d '{ "index": {"number_of_replicas": 0}}'
```

然后观察集群状态，最后通过命令在恢复期索引副本数据：

```bash
curl -u 'elastic:password' -H "Content-Type: application/json" -X PUT 'http://127.0.0.1:9200/<索引名称>/_settings' -d '{ "index": {"number_of_replicas": 1}}'
```

等待节点自动分配后，集群成功恢复成 green。