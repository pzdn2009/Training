# 简介

InfluxDB 是一个**开源分布式时序**、事件和指标数据库。

使用 Go 语言编写，无需外部依赖。其设计目标是实现分布式和水平伸缩扩展。

# 三大特性

1. Time Series （时间序列）：你可以使用与时间有关的相关函数（如最大，最小，求和等）
2. Metrics（度量）：你可以实时对大量数据进行计算
3. Eevents（事件）：它支持任意的事件数据

# 特点

  ● schemaless(无结构)，可以是任意数量的列
  ● Scalable
  ● min, max, sum, count, mean, median 一系列函数，方便统计
  ● Native HTTP API, 内置http支持，使用http读写
  ● Powerful Query Language 类似sql
  ● Built-in Explorer 自带管理工具

# 管理界面


# API
InfluxDB 支持两种api方式
  ● HTTP API
  ● Protobuf API