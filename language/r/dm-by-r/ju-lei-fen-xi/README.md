# 聚类分析

## 聚类分析

无监督学习的一种，将目标数据划分多个簇。

主要應用：

* 模式分析。将不同的模式给划分成不同的组。
* 數據可視化。将各组数据显示出来。
* 預處理。
* 離群點。找出异常点。

主要方法：

* KMeans
* KMedoids

## 1.特点

* 簇内：高紧凑性，compactness。

  WSS：Within Cluster Sums of Squares 簇內距離中心點平方和，選擇每個簇內的中心點。

* 簇间：高分离性，Separation。

  BSS：Between Cluster Sums of Squares簇間距離平方和，選擇樣本的均值作為中心點。

測量相似度——distance

* 離散變量：如欧式距離
* 類別變量：構造自己的距離

## 2. 计算

**WSS**

![](https://github.com/pzdn2009/Training/tree/5fba0130e1d3c85caf69075f68f15bb835326a99/assets/wss.png)

**BSS**

![](https://github.com/pzdn2009/Training/tree/5fba0130e1d3c85caf69075f68f15bb835326a99/assets/bss.png)

## 3.找到K：使WSS最小

![](https://github.com/pzdn2009/Training/tree/5fba0130e1d3c85caf69075f68f15bb835326a99/assets/totss.png)

拐点即为最适合的K：

![](https://github.com/pzdn2009/Training/tree/5fba0130e1d3c85caf69075f68f15bb835326a99/assets/screeploteblow.png)

