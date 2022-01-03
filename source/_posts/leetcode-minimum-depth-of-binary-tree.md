---
title: LeetCode第111题Minimum Depth of Binary Tree
date: 2019-10-10 13:14:13
updated: 
toc: true
top: 
categories: 
- 数据结构与算法
tags:
- LeetCode
- 树
- 二叉树
- 深度优先搜索
- 广度优先搜索
---
<!-- more -->
#### Minimum Depth of Binary Tree

[Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)求二叉树的最小深度。

##### Example:

![Lowest Common Ancestor of a Binary Search Tree Example1](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png)

>Input: root = [6,2,8,0,4,7,9,null,null,3,5]
>Output: 3

二叉树的深度可以通过深度优先搜索和广度优先搜索两种方式来遍历，并记录树的层次。

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
    // dfs
    public int minDepth(TreeNode root) {
        if (root == null) return 0;
        int left = minDepth(root.left);
        int right = minDepth(root.right);
        return (left == 0 || right ==0) ? left + right + 1 : Math.min(left, right) + 1;
    }
    // bfs
    public int minDepth(TreeNode root) {
        if (root == null) return 0;

        Deque<TreeNode> q = new LinkedList<>();
        q.add(root);
        int depth = 1;

        while (!q.isEmpty()){
            int size = q.size();
            while (size-- > 0){
                TreeNode node = q.poll();
                if (node.left == null && node.right == null){
                   return depth;
                }
                if (node.left != null){
                    q.offer(node.left);
                }
                if (node.right != null){
                    q.offer(node.right);
                }
            }
            depth++;
        }
        return depth;
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
    # dfs
    def minDepth(self, root: TreeNode):
        if not root:
            return 0
        if not root.left and not root.right:
            return 1
        if root.left and not root.right:
            return 1 + self.minDepth(root.left)
        if root.right and not root.left:
            return 1 + self.minDepth(root.right)
        return 1 + min(self.minDepth(root.left), self.minDepth(root.right))
    # bfs
    def minDepth(self, root):
        if not root: return 0
        q = collections.deque([(root, 1)])
        while q:
            curr, depth = q.popleft()
            if not curr.left and not curr.right:
                return depth
            if curr.left:
                q.append((curr.left, depth + 1))
            if curr.right:
                q.append((curr.right, depth + 1))
```

#### 总结

第111题是简单题，只需熟悉二叉树的深度优先搜索与广度优先搜索，并记录对应的层次即可。