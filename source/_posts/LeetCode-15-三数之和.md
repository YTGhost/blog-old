---
title: LeetCode-15.三数之和
date: 2020-06-12 15:23:14
tags:
- LeetCode
- 双指针
categories:
- 数据结构与算法
---
双指针题，两数之和进阶版

[15. 三数之和](https://leetcode-cn.com/problems/3sum/)
<!--more-->
我们可以先将数组进行从小到大的排序，然后遍历，以每个元素的负数作为目标值，这样就将三数之和问题变成了两数之和问题。

```c++
class Solution {
public:
    vector<vector<int>> ans;
    void towSum(vector<int>& nums, int begin, int end, int target, int value){
        while(begin < end)
        {
            int sum = nums[begin] + nums[end];
            if(sum == target){
                ans.push_back(vector<int>{value, nums[begin], nums[end]});
                while(begin < end && nums[begin] == nums[begin+1]){
                    begin++;
                }
                begin++;
                while(begin < end && nums[end] == nums[end-1]){
                    end--;
                }
                end--;
            }else if(sum > target){
                end--;
            }else{
                begin++;
            }
        }
    }

    vector<vector<int>> threeSum(vector<int>& nums) {
        int size = nums.size();
        sort(nums.begin(), nums.end());
        for(int i = 0; i < size; i++)
        {
            if(i > 0 && nums[i-1] == nums[i]) continue;
            towSum(nums, i+1, size-1, -nums[i], nums[i]);
        }
        return ans;
    }
};
```
