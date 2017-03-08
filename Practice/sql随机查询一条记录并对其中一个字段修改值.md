# SQL随机查询一条记录并对其中一个字段修改值

```sql
WITH T
AS
(
SELECT TOP 1 * FROM UId WHERE UsedTime IS NULL AND LockedTime IS NULL) ORDER BY newid()
)
UPDATE T SET LockedTime = getdate()
OUTPUT inserted.*

```

Ps:

1. order by newid();
2. WITH T
3. inserted