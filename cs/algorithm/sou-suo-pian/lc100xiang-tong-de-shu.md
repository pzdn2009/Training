# Lc100--相同的树

## 题目

https://leetcode-cn.com/problems/same-tree/

## 分析

按照前序遍历，如果相同就继续，如果不同，则返回false。

## submit

```java
package com.kolema.leetcode;

public class T100_ {

    public static void main(String[] args) {
        T100_ test = new T100_();

        TreeNode t1 = new TreeNode(1);
        t1.left = new TreeNode(2);
        t1.right = new TreeNode(3);

        TreeNode t2 = new TreeNode(1);
        t2.left = new TreeNode(2);
        t2.right = new TreeNode(3);

        System.out.println(test.isSameTree(t1, t2));


    }

    public boolean isSameTree(TreeNode p, TreeNode q) {

        if (p == null && q == null) {
            return true;
        }

        if (notSame(p, q)) {
            return false;
        }

        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }

    private boolean notSame(TreeNode p, TreeNode q) {
        return p == null && q != null || p != null && q == null || p.val != q.val;
    }

}

```