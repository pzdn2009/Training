# Lc053--最大子序和

## 题目

Ref：https://leetcode-cn.com/problems/maximum-subarray/

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:
```
输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

## 分析

连续是一个区间(i,j)，区间的组合复杂度在O(n^2)。

两步法：先计算出状态数组，再聚合结果。

* 状态：
```
d[i]表示以A[i]结尾时的最大连续和，d[i] = max{A[i], d[i-1] + A[i]};
```
* 结果：`max{d[0],,,d[n]}`


## submit

```java
package com.kolema.leetcode;

public class T053 {

    public static void main(String[] args) {
        T053 test = new T053();

        System.out.println(test.maxSubArray(new int[]{-2, 1, -3, 4, -1, 2, 1, -5, 4}));

    }


    public int maxSubArray(int[] nums) {

        //状态记录
        int[] dp = new int[nums.length];

        dp[0] = nums[0];
        int max = dp[0];

        for (int i = 1; i < nums.length; i++) {
            dp[i] = Math.max(nums[i], dp[i - 1] + nums[i]);
            if (dp[i] > max) {
                max = dp[i];
            }
        }
        return max;
    }
}
```