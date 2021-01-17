---
title: LeetCode-721.账户合并
date: 2021-01-18 01:00:09
tags:
- LeetCode
- 并查集
categories:
- 数据结构与算法
---
一道中等难度的并查集问题~
<!--more-->
题目链接:

[721. 账户合并](https://leetcode-cn.com/problems/accounts-merge/)

这道题可以分四步来做，大体的思路便是将每个accounts[i]作为一个节点，把具有相同归属的邮件的节点合并，再将合并后的节点中邮件去重，格式化为题目所需格式即可。

## 初始化
先初始化并查集所需的节点数组和路径压缩的find函数

## 合并节点
根据邮箱，将邮箱划分到不同节点之下。此时如果出现了相同邮箱归属不同节点，就把这俩节点进行合并操作

## 梳理邮箱归属
根据第二步合并节点的结果，将归属于具有相同根节点的节点下邮箱放到一起

## 按照题目要求格式整理输出
第三步做完其实就已经把每个账户中的邮箱整理好了，我们再根据根节点去拿name，最后排一下序就好了

```c++
class Solution {
public:
    /*第一步开始*/
    int p[1001];
    int find(int x) {
        if(p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        int n = accounts.size();
        for(int i = 0; i < n; i++) p[i] = i;
        /*第一步结束*/
        /*第二步开始*/
        unordered_map<string, int> m;
        for(int i = 0; i < n; i++)
            for(int j = 1; j < accounts[i].size(); j++)
            {
                if(m.count(accounts[i][j])) {
                    int root = m[accounts[i][j]];
                    p[find(root)] = find(i);
                }else {
                    m[accounts[i][j]] = i;
                }
            }
        /*第二步结束*/
        /*第三步开始*/
        unordered_map<int, unordered_set<string>> mm;
        for(int i = 0; i < n; i++)
        {
            int root = find(i);
            for(int j = 1; j < accounts[i].size(); j++)
                mm[root].insert(accounts[i][j]);
        }
        /*第三步结束*/
        /*第四步开始*/
        vector<vector<string>> res;
        for(auto node : mm)
        {
            vector<string> temp;
            int root = node.first;
            string name = accounts[root][0];
            temp.emplace_back(name);
            for(auto email : node.second)
                temp.emplace_back(email);
            // 按顺序排序
            sort(temp.begin(), temp.end());
            res.emplace_back(temp);
        }
        /*第四步结束*/
        return res;
    }
};
```