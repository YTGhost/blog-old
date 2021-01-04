---
title: LeetCode-面试题46.把数字翻译成字符串
date: 2020-06-09 11:34:59
tags:
- LeetCode
- 动态规划
categories:
- 数据结构与算法
---
基本DP问题

[面试题46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)
<!--more-->
这道题本质就是一个爬楼梯或者青蛙跳台阶问题：

[70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

[面试题10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

只不过是将跳台阶增加了条件，跳一阶台阶没有限制条件，但要跳两阶台阶则需要这两个字符在10到25之间，像09、07这样的不符合翻译规则。

DFS解法：
```c++
class Solution {
public:
    int ans;
    void dfs(string num, int count){
        if(count == num.size()){
            ans++;
            return;
        };
        if(count > num.size()) return;
        for(int i = 1; i <= 2; i++)
        {
            if(i == 2 && num[count] == '0') continue;
            if(stoi(num.substr(count, i)) <= 25) dfs(num, count+i);
        }
    }

    int translateNum(int num) {
        dfs(to_string(num), 0);
        return ans;
    }
};
```
滚动优化dp：
```c++
class Solution {
public:
    int translateNum(int num) {
        string str = to_string(num);
        int a = 0, b = 0, c = 1;
        for(int i = 0; i < str.length(); i++)
        {
            a = b;
            b = c;
            c = 0;
            c += b;
            if(i == 0) continue;
            if(str.substr(i-1, 2) >= "10" && str.substr(i-1, 2) <= "25") c += a;
        }
        return c;
    }
};
```

