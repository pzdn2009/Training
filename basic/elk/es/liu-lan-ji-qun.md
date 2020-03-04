# 浏览集群

## 浏览集群

## 1. 集群健康

```text
GET /_cat/heathy?v
```

輸出

```text
epoch      timestamp cluster           status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1482838116 19:28:36  acc_elasticsearch yellow          1         1     47  47    0    0       50             0                  -                 48.5%
```

```text
GET /_cat/nodes?v
```

輸出

```text
ip        heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
127.0.0.1           19          80   0    0.02    0.02     0.05 mdi       *      elk34
```

## 2. 索引

### 2.1 列出所有索引

```text
GET /_cat/indices?v
```

输出

```text
health status index               uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   logstash-2016.12.21 jwvAvfBJT9Gsta5bTjBPUw   5   1          3            0     12.7kb         12.7kb
yellow open   logstash-2016.12.22 oREHHh9YTwKVGJ5Hfhz3lw   5   1        869            0    681.4kb        681.4kb
yellow open   logstash-2016.12.20 O1nHmrI6TC6rMzKcwYB4XA   5   1         20            0     76.8kb         76.8kb
yellow open   .kibana             H0F837g8RoqSEsPpOQMrGQ   1   1          2            0      9.4kb          9.4kb
```

### 2.2 创建索引

```text
PUT /customer?pretty
```

輸出

```text
{
  "acknowledged": true,
  "shards_acknowledged": true
}
```

```text
GET /_cat/indices?v #查看索引
```

輸出

```text
health status index                   uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   logstash-2016.12.21     jwvAvfBJT9Gsta5bTjBPUw   5   1          3            0     12.7kb         12.7kb
yellow open   customer                ajbp5qrFQTKu_HW28Yc0jQ   5   1          0            0       650b           650b
```

## 2.3 索引和查詢

```text
PUT /customer/external/1?pretty
{
  "name": "John Doe"
}
```

輸出

```text
{
  "_index": "customer",
  "_type": "external",
  "_id": "1",
  "_version": 1,
  "result": "created",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "created": true
}
```

查詢：

```text
GET /customer/external/1?pretty
```

輸出

```text
{
  "_index": "customer",
  "_type": "external",
  "_id": "1",
  "_version": 1,
  "found": true,
  "_source": {
    "name": "John Doe"
  }
}
```

### 2.4 修改文檔

```text
POST customer/external/1
{
  "name":"pzdn2009"
}

POST /customer/external/1/_update?pretty
{
  "doc": { "name": "Jane Doe" }
}

POST /customer/external/1/_update?pretty
{
  "doc": { "name": "Jane Doe", "age": 20 }
}

POST /customer/external/1/_update?pretty
{
  "script" : "ctx._source.age += 5"
}
```

輸出

```text
{
  "_index": "customer",
  "_type": "external",
  "_id": "1",
  "_version": 2,
  "result": "updated",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "created": false
}

{
  "_index": "customer",
  "_type": "external",
  "_id": "1",
  "_version": 3,
  "result": "updated",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  }
}

{
  "_index": "customer",
  "_type": "external",
  "_id": "1",
  "_version": 4,
  "result": "updated",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  }
}

{
  "_index": "customer",
  "_type": "external",
  "_id": "1",
  "_version": 5,
  "result": "updated",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  }
}
```

可以看到數據版本變成了2，且created為false

### 2.5 刪除索引

```text
DELETE /customer?pretty

DELETE /customer/external/2?pretty
```

輸出

```text
{
  "acknowledged": true
}

{
  "found": false,
  "_index": "customer",
  "_type": "external",
  "_id": "2",
  "_version": 1,
  "result": "not_found",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  }
}
```

