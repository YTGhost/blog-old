---
title: LeetCode-126.单词接龙II
date: 2020-06-07 15:34:56
tags:
- LeetCode
- 哈希表
- BFS
categories:
- 数据结构与算法
---
这是一道比较综合的BFS题，难度在于建图和储存路线。

[126. 单词接龙 II](https://leetcode-cn.com/problems/word-ladder-ii/)
<!--more-->
两个单词之间的转变是只能有一个字母不同的，因此我们可以把这种转变看成是这两个单词之间的一条权重为1的无向边，而找出最短的转化序列即是找出一条权重最小的路径，便很显然使用BFS，但这里注意题目要求我们找出所有最短的，因此我们在BFS的时候每一次都得记录所有可以走的路径。
```c++
class Solution {
public:
    unordered_map<string, int> wordId;
    vector<string> idToWord;
    vector<vector<int>> edge;
    vector<vector<string>> ans;

    // 检查能否从一个单词变成另一个单词
    bool check(const string& str1, const string& str2){
        int diff = 0;
        for(int i = 0; i < str1.length(); i++)
        {
            if(str1[i] != str2[i]){
                if(++diff >= 2) return false;
            }
        }
        return true;
    }

    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        // 给单词设定id
        int id = 0;
        for(auto word : wordList)
        {
            if(!wordId.count(word)){
                wordId[word] = id++;
                idToWord.push_back(word);
            }
        }

        // 如果表中没有终点，显然转换不了
        if(!wordId.count(endWord)) return {};

        // 表中没有起点的话，加入起点
        if(!wordId.count(beginWord)){
            wordId[beginWord] = id++;
            idToWord.push_back(beginWord);
        }

        edge.resize(id);
        // 建图
        for(int i = 0; i < id; i++)
            for(int j = i+1; j < id; j++)
            {
                if(check(idToWord[i], idToWord[j])){
                    edge[i].push_back(j);
                    edge[j].push_back(i);
                }
            }
        
        queue<vector<int>> q;
        int dest = wordId[endWord];
        vector<int> dist(id, INT_MAX);
        q.push(vector<int> {wordId[beginWord]});
        dist[wordId[beginWord]] = 0;
        while(!q.empty())
        {
            vector<int> t = q.front();
            int back = t.back();
            q.pop();
            if(back == dest){
                vector<string> tmp;
                for(int index : t) tmp.push_back(idToWord[index]);
                ans.push_back(tmp);
            }else{
                for(int i = 0; i < edge[back].size(); i++)
                {
                    int to = edge[back][i];
                    if(dist[back] + 1 <= dist[to])
                    {
                        dist[to] = dist[back] + 1;
                        vector<int> tmp(t);
                        tmp.push_back(to);
                        q.push(tmp);
                    }
                }
            }
        }
        return ans;
    }
};
```

