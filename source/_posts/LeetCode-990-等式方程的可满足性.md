---
title: LeetCode-990.等式方程的可满足性
date: 2020-06-08 20:49:15
tags:
- LeetCode
- AcWing
- 并查集
categories:
- 数据结构与算法
---
这道题本质就是并查集的基本应用，需要了解如何使用并查集。

[990. 等式方程的可满足性](https://leetcode-cn.com/problems/satisfiability-of-equality-equations/)
<!--more-->
对于这道题，元素之间有两种情况，要么相等要么不等。我们可以找出方程中相等的方程，将相等的元素都放入同一个集合中。之后再来判断不相等的情况，若是两个元素不相等则它们肯定是在两不同的集合中。因此这道题便转换成了合并集合和判断元素是否在某一个集合中的问题。
对于这一类问题显然使用并查集。
关于并查集的操作可以参考下面这道题：
[836. 合并集合](https://www.acwing.com/problem/content/838/)
```c++
class Solution {
public:
    int p[26];
    int find(int x){
        if(p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    bool equationsPossible(vector<string>& equations) {
        for(int i = 0; i < equations.size(); i++)
        {
            int a = equations[i][0] - 'a';
            int b = equations[i][3] - 'a';
            if(p[a] == 0) p[a] = a;
            if(p[b] == 0) p[b] = b;
            if(equations[i][1] == '=') p[find(a)] = find(b);
        }
        for(int i = 0; i < equations.size(); i++)
        {
            int a = equations[i][0] - 'a';
            int b = equations[i][3] - 'a';
            if(equations[i][1] == '!' && find(a) == find(b)) return false;
        }
        return true;
    }
};
```


