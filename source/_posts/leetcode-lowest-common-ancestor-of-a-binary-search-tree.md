---
title: LeetCode第235题Lowest Common Ancestor of a Binary Search Tree
date: 2019-10-09 10:23:53
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
#### Lowest Common Ancestor of a Binary Search Tree

[Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)给定一个二叉**搜索**树，找到树中两个给定结点的最近公共祖先结点[LCA](https://en.wikipedia.org/wiki/Lowest_common_ancestor)。

##### Example

![Lowest Common Ancestor of a Binary Search Tree Example1](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png)

>Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
>Output: 6
>Explanation: The LCA of nodes 2 and 8 is 6.

这一题和236题[Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)有些相似，二叉搜索树是二叉树的一种，所以236题的解法也适用于这题。特别的是，二叉搜索树中根节点的值大于左子树的节点值，小于右子树的节点值，这是可以利用的一个性质：

- 我们记`LCA(2,8) = 6`，此时节点`p`和`q`是2和8，我们将节点值与根节点值6进行比较，发现`p`和`q`分别位于根节点左右两侧，此时根节点6是最近祖先。
- 我们记`LCA(7,9) = 8`，此时节点`p`和`q`是7和9，我们将节点值与根节点值6进行比较，发现`p`和`q`都大于根节点，都位于右子树，于是，我们进入右子树进行遍历，此时原来右子树的根节点8成为新的根节点，继续遍历比较。

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
        int parentVal = root.val;
        int pVal = p.val;
        int qVal = q.val;

        if (pVal > parentVal && qVal > parentVal) {
            return lowestCommonAncestor(root.right, p, q);
        } else if (pVal < parentVal && qVal < parentVal) {
            return lowestCommonAncestor(root.left, p, q);
        } else {
            return root;
        }
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
    def lowestCommonAncestor(self, root, p, q):
        if p.val < root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        if p.val > root.val < q.val:
            return  self.lowestCommonAncestor(root.right, p, q)
        return  root
```

#### 总结

第235题是简单题，利用二叉**搜索**树的性质，通过递归可以写出美感较强的代码，易于阅读。