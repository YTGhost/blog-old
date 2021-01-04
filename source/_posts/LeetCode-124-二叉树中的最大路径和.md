---
title: LeetCode-124.二叉树中的最大路径和
date: 2020-06-21 09:41:14
tags:
- 树
- DFS
categories:
- 数据结构与算法
---
关于二叉树的一道题

[124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)
<!--more-->
对于每一个节点，这里的路径可以有两种：
- 左右子树都在最大路径中，最大路径为左子树->节点->右子树
- 节点作为父节点的左子树或右子树，该节点的值加上其左右子树中路径和较大的那个，回溯给父节点。

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxLength = INT_MIN;
    int getMax(TreeNode* root){
        if(!root) return 0;
        int left = max(0, getMax(root->left));
        int right = max(0, getMax(root->right));
        maxLength = max(maxLength, root->val + left + right);
        return root->val + max(left, right);
    }

    int maxPathSum(TreeNode* root) {
        getMax(root);
        return maxLength;
    }
};
```

