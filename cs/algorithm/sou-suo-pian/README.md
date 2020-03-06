# 搜索篇

## 搜索篇

关键词： 搜索树、部分解、死节点。 DFS搜索树、树边、回边、

## 回溯法BackTracking

发现原先选择并不优或达不到目标，就退回一步重新选择，这种走不通就退回再走的技术为回溯法，而满足回溯条件的某个状态的点称为“回溯点”。

* 3着色
* 8皇后

## 深度优先搜索DFS

图G = \(V,E\)

1. 每个V标记为未访问；
2. 对每个V，如果未访问，则dfs\(v\)

DFS\(v\): 1. 对于\(v,w\)，如果w未访问，则dfs\(w\)；

应用：

* 图的回路
* 拓扑排序
* 图的关节点

## 广度优先搜索BFS

图G = \(V,E\)

1. 每个V标记为未访问；
2. 对每个V，如果未访问，则bfs\(v\)

BFS\(v\)：

1. v入队，标记为已访问；
2. 队列不空，出队，每条边\(v,w\)，if w 未访问，入队，标记w为已访问；
3. 直到队列空为止。
