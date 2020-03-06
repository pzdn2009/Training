# Lc104--二叉树的最大深度

## 分析

一开始采用BFS来做，但是提交了之后，结果并不好。

后面改为DFS

## submit

```java
package com.kolema.leetcode;

import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.LinkedList;
import java.util.Queue;

public class T104 {

    public static void main(String[] args) {
        T104 test = new T104();
    }


    public int maxDepth(TreeNode root) {

        if (root == null) {
            return 0;
        }

        if (root.left == null && root.right == null) {
            return 1;
        }

        return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
    }

    int bfs(TreeNode root) {

        Queue<TreeNode> queue = new LinkedList();
        queue.add(root);

        HashMap<TreeNode, Integer> visitLevel = new LinkedHashMap<>();
        visitLevel.put(root, 1);

        int res = 0;

        while (!queue.isEmpty()) {
            TreeNode top = queue.remove();
            int level = visitLevel.get(top);

            if (level > res) {
                res = level;
            }
            if (top.left != null) {
                queue.add(top.left);
                visitLevel.put(top.left, level + 1);
            }
            if (top.right != null) {
                queue.add(top.right);
                visitLevel.put(top.right, level + 1);
            }
        }

        return res;
    }
}
```

