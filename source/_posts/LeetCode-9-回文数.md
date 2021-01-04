---
title: LeetCode-9.回文数
date: 2020-06-10 08:16:42
tags:
- LeetCode
- 数学
categories:
- 数据结构与算法
---
回文数，可以不把数字转换为字符串，也可以转换为字符串来做。

[9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)
<!--more-->
首先回文数一定不是负数，所以我们在开头便可以把负数直接return false

转换为字符串的解法的话便是先转换为字符串，然后双指针一个从左到右，一个从右到左来遍历
```c++
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0) return false;
        string str = to_string(x);
        int l = 0, r = str.length() - 1;
        for( ; l < r; l++, r--)
        {
            if(str[l] != str[r]) return false;
        }
        return true;
    }
};
```
如果不能转换为字符串的话，也可以利用回文数的自身性质：从左到右和从右到左遍历得到的数是相同的。即例如121，我们可以把个位上的1放到百位上，百位上的1放到个位上，其结果和原来的值是相同的。
```c++
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0) return false;
        int temp = x;
        long sum = 0;
        while(x)
        {
            sum = sum * 10 + x % 10;
            x /= 10;
        }
        return sum == temp ? true : false;
    }
};
```


