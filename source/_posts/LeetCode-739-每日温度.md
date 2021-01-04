---
title: LeetCode-739.每日温度
date: 	2020-06-11 23:56:56
tags:
- LeetCode
- 单调栈
categories:
- 数据结构与算法
---
单调栈的基本应用

[739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)
<!--more-->
从正向遍历，若是栈为空或者当前温度小于等于前一天的温度就入栈；当前温度大于栈顶温度则将栈顶出栈，同时更新栈顶对应的等待天数为当前温度所在日期减去栈顶温度所在日期，循环直到当前温度不再大于栈顶温度，此时入栈。这样就能保证每一个日期更新都是最小的等待时间了。

```c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        int size = T.size();
        vector<int> ans(size);
        stack<int> s;
        for(int i = 0; i < size; i++)
        {
            while(!s.empty() && T[i] > T[s.top()])
            {
                int t = s.top();
                s.pop();
                ans[t] = i - t;
            }
            s.push(i);
        }
        return ans;
    }
};
```
