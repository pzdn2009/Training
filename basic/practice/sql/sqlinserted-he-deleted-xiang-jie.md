# SQL Inserted和deleted详解

## 原理

**Deleted 表**用于存储 DELETE 和 UPDATE 语句所影响的行的复本。 在执行 DELETE 或 UPDATE 语句时，行从触发器表中删除，并传输到 deleted 表中。 Deleted 表和触发器表通常没有相同的行。

**Inserted 表**用于存储 INSERT 和 UPDATE 语句所影响的行的副本。在一个插入或更新事务处理中，新建行被同时添加到 inserted 表和触发器表中。 Inserted 表中的行是触发器表中新行的副本。

1. 插入操作（Insert） Inserted表有数据，Deleted表无数据
2. 删除操作（Delete） Inserted表无数据，Deleted表有数据
3. 更新操作（Update） Inserted表有数据（新数据），Deleted表有数据（旧数据）

