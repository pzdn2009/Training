# Index API 

索引API添加或者更新一个类型JSON文档到一个索引，使它可以搜索。

```
# 不会检测重复，版本号递增
PUT twitter/tweet/2
{
  "user":"pzdn",
  "post_date": "2009-11-15T14:12:12",
  "message":"trying out Elasticsearch"
}

# 会检查是否存在，报409
PUT twitter/tweet/2/_create
{
  "user":"pzdn",
  "post_date": "2009-11-15T14:12:12",
  "message":"trying out Elasticsearch"
}

# 会检查是否存在，报409
PUT twitter/tweet/2?op_type=create
{
  "user":"pzdn",
  "post_date": "2009-11-15T14:12:12",
  "message":"trying out Elasticsearch"
}
```

Shard解释：提供了索引过程中，副本的处理结果。
```
"_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
```