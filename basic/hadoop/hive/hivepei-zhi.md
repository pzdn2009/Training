# Hive配置

Ref:https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties

对于一般参数，有以下三种设定方式：

* 配置文件
* 命令行参数
* 参数声明

# 1. 配置文件：Hive的配置文件包括

* 用户自定义配置文件：`$HIVE_CONF_DIR/hive-site.xml`
* 默认配置文件：`$HIVE_CONF_DIR/hive-default.xml`

Hive也会读入Hadoop的配置，因为Hive是作为Hadoop的客户端启动的，Hive的配置会覆盖Hadoop的配置。

配置文件的设定对本机启动的所有Hive进程都有效。

# 2. 命令行参数

启动Hive（客户端或Server方式）时，可以在命令行添加`-hiveconf param=value`来设定参数，例如：
```
bin/hive -hiveconf hive.root.logger=INFO,console
```
这一设定对本次启动的Session（对于Server方式启动，则是所有请求的Sessions）有效。

# 3. 参数声明

可以在HQL中使用`SET`关键字设定参数，例如：
```
set mapred.reduce.tasks=100;
```
这一设定的作用域也是session级的。

上述三种设定方式的优先级依次递增。


