# RDD-transform

Ref：http://homepage.cs.latrobe.edu.au/zhe/ZhenHeSparkRDDAPIExamples.html

* map(func)：高阶，每个元素，返回新RDD。
* flatMap(func)：高阶，每个元素返回一个序列，扁平化，返回新RDD。
* mapPartitions(func)：高阶，
* filter(func)：高阶，布尔函数，每个元素，返回新RDD。

* sample\(withReplacement,fraction,\[seed\]\)
* [union：并集，distinct：去重，intersection：交集，subtract：差集
cartesian：笛卡尔积](/deco/spark/rddbian-cheng/rdd-transform/rdd-transform-collection.md)

* [zipWithIndex,zipWithUniqueId,zip](/deco/spark/rddbian-cheng/rdd-transform/rdd-transform-zip.md)：元组形式连接
* [groupByKey](/deco/spark/rddbian-cheng/rdd-transform/rdd-transform-groupby.md)([_numTasks_])：在一个 (K, V) pair 的 dataset 上调用时，返回一个`(K, Iterable<V>)` .
* [keyBy](/deco/spark/rddbian-cheng/rdd-transform/rdd-transform-keyby.md)
* [sortBy](/deco/spark/rddbian-cheng/rdd-transform/rdd-transform-sortby.md)
* pipe
* sample





