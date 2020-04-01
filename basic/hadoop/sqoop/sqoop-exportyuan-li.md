# Sqoop-export原理


# 1. sqoop-export 语法

```
$ sqoop export (generic-args) (export-args)
$ sqoop-export (generic-args) (export-args)
```

控制参数：

Argument|Description
--|--
--columns <col,col,col…>	|Columns to export to table 要导出到表的列
--direct	|Use direct export fast path
--export-dir <dir>	|HDFS source path for the export 文件夹
-m,--num-mappers <n>	|Use n map tasks to export in parallel
--table <table-name>	|Table to populate
--call <stored-proc-name>	|Stored Procedure to call
--update-key <col-name>|	Anchor column to use for updates. Use a comma separated list of columns if there are more than one column. 用于更新的键
--update-mode <mode>	|Specify how updates are performed when new rows are found with non-matching keys in database.Legal values for mode include `updateonly` (default) and `allowinsert`.
--input-null-string <null-string>	|The string to be interpreted as null for string columns
--input-null-non-string <null-string>|	The string to be interpreted as null for non-string columns
--staging-table <staging-table-name>	|The table in which data will be staged before being inserted into the destination table.
--clear-staging-table|	Indicates that any data present in the staging table can be deleted.
--batch|	Use batch mode for underlying statement execution.


