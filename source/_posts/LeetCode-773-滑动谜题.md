---
title: LeetCode-773.滑动谜题
date: 2020-06-06 18:41:21
tags:
- LeetCode
- 哈希表
- BFS
- AcWing
categories:
- 数据结构与算法
---
这道题其实就是经典的BFS问题——八数码

[LeetCode 773. 滑动谜题](https://leetcode-cn.com/problems/sliding-puzzle/)

[AcWing 845. 八数码](https://www.acwing.com/problem/content/847/)
<!--more-->
该问题的难点在于怎么表示状态和路程，这里可以使用哈希表来存储到每个状态的路程长度，并将二维数组的八数码转换为一维形式，这里我转换为了string。之后就是正常的入队出队，枚举上下左右并看看有没有出界，改变状态（swap），如果是一个全新的状态，便在之前状态路程的基础上+1并入队，之后再恢复状态。

LeetCode
```c++
class Solution {
public:
    int bfs(string start){
        unordered_map<string, int> d;
        queue<string> q;
        string end = "123450";
        int px[] = {1, 0, -1, 0}, py[] = {0, -1, 0, 1};
        q.push(start);
        d[start] = 0;
        while(!q.empty())
        {
            string t = q.front();
            int distance = d[t];
            q.pop();
            if(t == end) return d[t];
            int k = t.find('0');
            int a = k % 3, b = k / 3;
            for(int i = 0; i < 4; i++)
            {
                int newX = a + px[i];
                int newY = b + py[i];
                if(newX >= 0 && newX < 3 && newY >= 0 && newY < 2){
                    swap(t[k], t[newY*3 + newX]);
                    if(!d.count(t)){
                        d[t] = distance + 1;
                        q.push(t);
                    }
                    swap(t[k], t[newY*3 + newX]);
                }
            }
        }
        return -1;
}
    int slidingPuzzle(vector<vector<int>>& board) {
        string start;
        for(int i = 0; i < 2; i++)
            for(int j = 0; j < 3; j++)
                start += to_string(board[i][j]);
        return bfs(start);
    }
};
```
AcWing
```c++
#include <iostream>
#include <algorithm>
#include <unordered_map>
#include <queue>
using namespace std;

int bfs(string start)
{
    unordered_map<string, int> d;
    queue<string> q;
    string end = "12345678x";
    int px[] = {1, 0, -1, 0}, py[] = {0, -1, 0, 1};
    q.push(start);
    d[start] = 0;
    while(!q.empty())
    {
        string t = q.front();
        int distance = d[t];
        q.pop();
        if(t == end) return d[t];
        int k = t.find('x');
        int a = k % 3, b = k / 3;
        for(int i = 0; i < 4; i++)
        {
            int newX = a + px[i];
            int newY = b + py[i];
            if(newX >= 0 && newX < 3 && newY >= 0 && newY < 3){
                swap(t[k], t[newY*3 + newX]);
                if(!d.count(t)){
                    d[t] = distance + 1;
                    q.push(t);
                }
                swap(t[k], t[newY*3 + newX]);
            }
        }
    }
    return -1;
}

int main()
{
    string start;
    for(int i = 0; i < 9; i++)
    {
        char c;
        cin >> c;
        start += c;
    }
    cout << bfs(start) << endl;
    return 0;
}
```
