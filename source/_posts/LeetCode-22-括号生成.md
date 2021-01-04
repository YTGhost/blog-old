---
title: LeetCode-22.括号生成
date: 2020-04-09 19:41:15
tags:
- LeetCode
- DFS
categories:
- 数据结构与算法
---
题目链接：
[22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

这道的思路也是比较清晰的，用DFS就行啦。
<!--more-->
输入的n可以代表左括号的个数，另设一个变量m表示此时可用的右括号个数。
1. 当n和m都为零时，意味着我们已经构建好了括号组合，输出即可
2. 当m等于零时，只能选择左括号放入
3. 当m不等于零且n不等于零时，可以选择左括号放入，也可以选择右括号放入
4. 当n等于零且m不等零时，只能选择右括号放入

这样我们就找出了在dfs中可能的所有选择，因此便可得到下述代码了：
```c++
class Solution {
public:
    vector<string> s;
    vector<string> generateParenthesis(int n) {
        string t = "";
        dfs(n, 0, t);
        return s;
    }

    void dfs(int n, int m, string t){
        if(n == 0 && m == 0){
            s.push_back(t);
            return;
        }
        if(m == 0) dfs(n-1, m+1, t+'(');
        else if(n != 0){
            dfs(n-1, m+1, t+'(');
            dfs(n, m-1, t+')');
        }else{
            dfs(n, m-1, t+')');
        }
    }
};
```

