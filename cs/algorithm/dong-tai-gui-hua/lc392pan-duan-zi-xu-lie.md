# Lc392--判断子序列

Ref：[https://leetcode-cn.com/problems/is-subsequence/](https://leetcode-cn.com/problems/is-subsequence/)

## 题目

```text
给定字符串 s 和 t ，判断 s 是否为 t 的子序列。

你可以认为 s 和 t 中仅包含英文小写字母。字符串 t 可能会很长（长度 ~= 500,000），而 s 是个短字符串（长度 <=100）。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。

示例 1:
s = "abc", t = "ahbgdc"

返回 true.

示例 2:
s = "axc", t = "ahbgdc"

返回 false.

后续挑战 :

如果有大量输入的 S，称作S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？
```

## 分析

一开始使用递推，打表，得到转移方程：

```text
dp[i,j]表示(s[i],t[j])时的子串长度。
结果：dp[m,n] >= s.length;
状态转移：dp[i,j] = s[i-1] == t[j-1] ? 1 + dp[i-1,j-1] : max { dp[i-1,j]，dp[i,j-1] }
```

耗时超过一秒。

降维处理：

```text
dp[i]=k，k表示地i个字母在字符串中的位置。
dp[i+1] = l 且 l > k，反之，false。
```

## submit

```java
package com.kolema.leetcode;

public class T392 {

    public static void main(String[] args) {
        T392 test = new T392();

        System.out.println(test.isSubsequence("abc", "ahbgdc"));
        System.out.println(test.isSubsequence("axc", "ahbgdc"));
    }


    public boolean isSubsequence2(String s, String t) {

        int m = s.length();
        int n = t.length();

        int[][] states = new int[m + 1][n + 1];
        for (int i = 0; i < m + 1; i++) {
            for (int j = 0; j < n + 1; j++) {
                states[i][j] = 0;
            }
        }

        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {

                char sChar = s.charAt(i - 1);
                char tChar = t.charAt(j - 1);

                if (sChar == tChar) {
                    states[i][j] = 1 + states[i - 1][j - 1];
                } else {
                    states[i][j] = Math.max(states[i - 1][j - 1], Math.max(states[i - 1][j], states[i][j - 1]));
                }
            }
        }

        return states[m][n] >= s.length();
    }

    public boolean isSubsequence(String s, String t) {

        int lastIndex = -1;
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            int index = t.indexOf(ch, lastIndex + 1);
            if (index > lastIndex) {
                lastIndex = index;
            } else {
                return false;
            }
        }

        return true;
    }
}
```

