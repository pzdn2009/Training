# Lc121--卖卖股票的最佳时机

Ref: [https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

## 题目

```text
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

示例 1:

输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
示例 2:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

## 分析

遍历数组，都会得到一个状态值，最后获取最大即可。

卖出的最大收益：0,0,4,2,5,3

状态转移方程：在0和差之间选择。 `dp[i] = max{0, A[i] - min{A[i-1]} };`

结果： `res = max { dp[i]}`

## submit

```java
package com.kolema.leetcode;

public class T121 {

    public static void main(String[] args) {
        T121 test = new T121();

        System.out.println(test.maxProfit(new int[]{7, 1, 5, 3, 6, 4}));
        System.out.println(test.maxProfit(new int[]{7, 1}));
        System.out.println(test.maxProfit(new int[]{7}));
        System.out.println(test.maxProfit(new int[]{}));
    }


    public int maxProfit(int[] prices) {

        int min = Integer.MAX_VALUE;
        int max = 0;

        for (int i = 0; i < prices.length; i++) {

            max = Math.max(max, Math.max(prices[i] - min, 0));

            min = Math.min(min,prices[i]);
        }

        return max;

    }
}
```

