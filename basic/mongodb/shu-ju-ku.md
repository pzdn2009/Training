# 數據庫

一个MongoDB中可以建立多个数据库。

MongoDB的默认数据库为"db"，该数据库存储在data目录中。

一些基礎操作：

```text
# 查看數據庫列表
> show dbs 
SchedulerDB         0.074GB
VisaMongoDB         0.000GB
local               1.923GB

# 顯示當前數據庫
> db
InfDB

# 切換或者創建數據庫
> use Market 
switched to db Market, or create db.

# 插入文檔
> db.Names.insert({"name":"pzdn2009"})
Inserted 1 record(s) in 13ms

# 刪除數據庫
> db.dropDatabase()

/* 1 */
{
    "dropped" : "Market",
    "ok" : 1.0
}
```

