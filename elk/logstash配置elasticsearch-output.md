# Logstash配置elasticsearch

# 1.主要字段
- hosts
类型是数组，默认值为["localhost:9200"]
- index
类型是string，默认值为"logstash-%{+YYYY.MM.dd}"，
- action
类型是string，默认值为index。合法的action
 - index:索引一个文档。indexes a document (an event from Logstash).
 - delete: 通过id删除一个文档。deletes a document by id (An id is required for this action)
 - create: 索引一个文档，如果文档已经存在就会失败。indexes a document, fails if a document by that id already exists in the index.
 - update: 根据id更新文档。updates a document by id. 
- document_id 
文档id
- document_type
文档类型


# 2.示例配置

```
elasticsearch { hosts => ["localhost:9200"] }
elasticsearch { hosts => ["127.0.0.1:9200","127.0.0.2:9200"] }
```