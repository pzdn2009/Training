# 基础概念

## ES基础概念

## 1. NRT

Near RealTime, Elasticsearch 是接近实时搜索的，大概有1秒的延迟。

## 2. Cluster

集群由多个节点构成。一个节点只能隶属于一个集群。

集群有个唯一的名称，默认为elasticsearch

示例集群名称：

logging-dev, logging-stage, and logging-prod

## 3. Node

节点代表一台服务器，用UUID唯一标识。

## 4. Index

具有相同特征的文档的集合。

一个集群中可以定义多个索引。

类似 关系数据库的DB。

## 5. Type

一个索引中，可以定义多个类型，是对索引的逻辑划分。通常，类型为文档提供公共的字段定义。

类型 关系数据库的表结构。

## 6. Document

被索引的基本单元。Json格式的数据。

类型关系数据库的记录。

## 7. Shards & Replias

分片和备份。分别用来存储大数据和做高可用。

