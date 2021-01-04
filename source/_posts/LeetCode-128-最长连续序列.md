---
title: LeetCode-128.最长连续序列
date: 2020-06-06 07:50:09
tags:
- LeetCode
- 哈希表
categories:
- 数据结构与算法
---
题目链接:

[128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

本题要求的时间复杂度为O(n)，需要用哈希表来以空间换时间
<!--more-->
思路：哈希表中存储数组中的每一个数（去重），再遍历一遍数组，当遍历到的数为一段序列的第一个元素时，即可开始匹配是否有后续元素，同时更新长度即可。
```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> s;
        int ans = 0;
        for(auto num : nums) s.insert(num); // 哈希表去重
        for(auto num : nums)
        {
            if(!s.count(num-1)){    // 当为一段序列的第一个元素时
                int cur = num;
                int length = 1;
                while(s.count(++cur)) length++; // 有后续元素时，更新长度
                ans = max(ans, length);
            }
        }
        return ans;
    }
};
```
复杂度分析：
- 时间复杂度：O(n)
- 空间复杂度：O(n)
