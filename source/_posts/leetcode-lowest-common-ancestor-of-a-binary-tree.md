---
title: LeetCode第236题Lowest Common Ancestor of a Binary Tree
date: 2019-09-30 08:19:18
updated: 
toc: true
top: 
categories: 
- 数据结构与算法
tags:
- LeetCode
- 树
- 二叉树
---
<!-- more -->
#### Lowest Common Ancestor of a Binary Tree

[Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)给定一个二叉树，找到树中两个给定结点的最近公共祖先结点[LCA](https://en.wikipedia.org/wiki/Lowest_common_ancestor)。

##### Example

![Lowest Common Ancestor of a Binary Tree Example1](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

>Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
>Output: 3
>Explanation: The LCA of nodes 5 and 1 is 3.

如何寻找最近公共祖先呢，我们可以通过遍历目标结点`q`和`p`的路径，得到其中重合的最近的部分，这很直接，比较好理解。这里我们使用递归的方式来遍历，从根节点开始分别遍历左右子树，如果目标结点仅存在于左子树内，那么最近公共节点必然位于左子树内，如果目标结点同时存在于左右子树内，那么最近公共节点必然是根节点。

#### Solution

下面是`Java`的代码：

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        return left == null ? right : right == null ? left : root;
    }
}
```

下面是`Python`的代码：

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def lowestCommonAncestor(self, root, p, q)
        if root in {None, p, q}:
            return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        return root if (left and right) else (left or right)
```

#### 总结

第236题是中等题，利用二叉树的性质，通过递归可以写出美感较强的代码，易于阅读。