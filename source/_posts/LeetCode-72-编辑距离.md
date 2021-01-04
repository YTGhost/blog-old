---
title: LeetCode-72.编辑距离
date: 2020-04-07 00:08:21
tags: 
- 动态规划 
- LeetCode
categories:
- 数据结构与算法
---

题目链接：
[72.编辑距离](https://leetcode-cn.com/problems/edit-distance/)

今天的这道每日一题是较为典型的动态规划问题，我们可以开一个二维的dp数组来记录状态，具体如下：

<!--more-->

## 状态定义

$$
dp[i][j]为word1前i个字母转换为word2前j个字母所需的最少操作数
$$

## 状态转移

$$
dp[i][j] = min(dp[i-1][j], min(dp[i][j-1], dp[i-1][j-1])) + 1
$$

插入一个字符就相当于是word1前i个转换为word2前j-1个所需最少操作数再加1（前i个已经转换为j-1个了，再加上word2的第j个就好了）

删除一个字符就相当于是word1前i-1个转换为word2前j个所需最少操作数再加1（前i-1都成功转换了，那么第i个直接删掉就好）

替换一个字符就相当于word1前i-1个转换为word2前j-1个所需最少操作数再加1（word1前i-1已经跟word2前j-1相同了，再把word1的第i个替换成word2的第j个就好了）

分析完我们就可以得到下面的代码啦：

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n = word1.size();
        int m = word2.size();
        int dp[n+1][m+1];
        
        for(int i = 0; i <= n; i++) dp[i][0] = i;
        for(int j = 0; j <= m; j++) dp[0][j] = j;

        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= m; j++)
            {
                if(word1[i-1] == word2[j-1]) dp[i][j] = dp[i-1][j-1];
                // dp[i-1][j]是删除，dp[i][j-1]是插入,dp[i-1][j-1]是替换
                else dp[i][j] = min(dp[i-1][j], min(dp[i][j-1], dp[i-1][j-1])) + 1;
            }
        return dp[n][m];
    }
};
```

