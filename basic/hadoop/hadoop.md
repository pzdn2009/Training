# 权威指南笔记

第二章 关于MapReduce

1. 半结构化，记录方式的数据? \(semi-structured data\)。介于完全结构化数据（如关系型数据库、面向对象数据库中的数据）和完全无结构的数据（如声音、图像文件等）之间的数据，HTML文档就属于半结构化数据。半结构化数据：树、图。 存储方式一般为：化为结构化存储，或者存为XML
2. NCDC数据获取，大体的格式？ [http://www.ncdc.noaa.gov/](http://www.ncdc.noaa.gov/) [http://hadoopbook.com/code.html](http://hadoopbook.com/code.html) [https://github.com/tomwhite/hadoop-book/tree/master/input/ncdc/all](https://github.com/tomwhite/hadoop-book/tree/master/input/ncdc/all) gunzip -c 1901.gz &gt; 1901data 0029029070999991901010320004+64333+023450FM-12+000599999V0202301N011819999999N0000001N9-00281+99999101741ADDGF108991999999999999999999
3. 该数据集中每年全球气温的最高记录是多少？ 找到气温的字段，一个文件一个文件地分析，然后取最大值，打印。

