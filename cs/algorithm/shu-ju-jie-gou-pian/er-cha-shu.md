# 二叉树

树：

* 一棵树中任意两个结点有且仅有唯一的一条路径连通
* 一棵树如果有n个结点，那它一定恰好有n-1条边
* 在一棵树中加一条边将会构成一个回路
* 树中有且仅有一个没有前驱的结点称为根结点

满二叉树：二叉树中每个内部结点都有存在左子树和右子树（或者说满二叉树所有的叶结点都有同样的深度）

满二叉树的严格的定义是：一颗深度为h且有2^\(h-1\)个结点的二叉树.

层次：根结点的层次为1，其余结点的层次等于该结点的双亲结点的层次加1

树的高度：树中结点的最大层次

前序遍历：首先访问根结点，然后遍历左子树，最后遍历右子树（根-&gt;左-&gt;右） 中序遍历：首先遍历左子树，然后访问根节点，最后遍历右子树（左-&gt;根-&gt;右） 后序遍历：首先遍历左子树，然后遍历右子树，最后访问根节点（左-&gt;右-&gt;根）

存储：数组和链表。

