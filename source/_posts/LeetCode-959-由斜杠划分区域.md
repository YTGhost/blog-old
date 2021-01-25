---
title: LeetCode-959.由斜杠划分区域
date: 2021-01-25 16:18:49
tags:
- LeetCode
- 并查集
categories:
- 数据结构与算法
---
稍微拐拐弯的并查集问题~

[959. 由斜杠划分区域](https://leetcode-cn.com/problems/regions-cut-by-slashes/)
<!--more-->
这道题转换一下可知是一个求连通块数量的问题，通过将每个1×1的小方块通过对角线切分成4块，再使用并查集连同起来即可。
```c++
class Solution {
public:
    int p[3610];
    int find(int x) {
        if(p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    void merge(int x, int y) {
        p[find(x)] = find(y);
    }
    int regionsBySlashes(vector<string>& grid) {
        int n = grid.size();
        for(int i = 0; i < n*n*4; i++) p[i] = i;

        for(int i = 0; i < n; i++)
            for(int j = 0; j < n; j++)
            {
                int index = i * n + j;
                // 最后一行就没有bottom了
                if(i < n-1) {
                    int bottom = index + n;
                    merge(index * 4 + 2, bottom * 4);
                }
                // 最后一列就没有right了
                if(j < n-1) {
                    int right = index + 1;
                    merge(index * 4 + 1, right * 4 + 3);
                }
                if(grid[i][j] == '/') {
                    merge(index * 4 + 1, index * 4 + 2);
                    merge(index * 4, index * 4 + 3);
                } else if(grid[i][j] == '\\') {
                    merge(index * 4, index * 4 + 1);
                    merge(index * 4 + 2, index * 4 + 3);
                } else {
                    merge(index * 4, index * 4 + 1);
                    merge(index * 4 + 1, index * 4 + 2);
                    merge(index * 4 + 2, index * 4 + 3);  
                }
            }
        int res = 0;
        for(int i = 0; i < n * n * 4; i++)
            if(p[i] == i) res++;
        return res;
    }
};
```