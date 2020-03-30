# HiveDML-Update&Delete&Merge

# 1. Update

```sql
Standard Syntax:
UPDATE tablename SET column = value [, column = value ...] [WHERE expression]
```

# 2. Delete

```sql
Standard Syntax:
DELETE FROM tablename [WHERE expression]
```

# 3. Merge

Merge can only be performed on tables that support ACID. See Hive Transactions for details.

```sql
Standard Syntax:
MERGE INTO <target table> AS T USING <source expression/table> AS S
ON <boolean expression1>
WHEN MATCHED [AND <boolean expression2>] THEN UPDATE SET <set clause list>
WHEN MATCHED [AND <boolean expression3>] THEN DELETE
WHEN NOT MATCHED [AND <boolean expression4>] THEN INSERT VALUES<value list>
````