---
title: LeetCode第104题Maximum Depth of Binary Tree
date: 2019-10-11 15:12:39
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
#### Maximum Depth of Binary Tree

[Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)求二叉树的最大深度。

##### Example:

![Lowest Common Ancestor of a Binary Search Tree Example1](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png)

>Input: root = [6,2,8,0,4,7,9,null,null,3,5]
>Output: 4

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
    public int maxDepth(TreeNode root) {
        return root == null ? 0 : 1 + Math.max(maxDepth(root.left),maxDepth(root.right));
    }
    // bfs
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;

        Deque<TreeNode> q = new LinkedList<>();
        q.add(root);
        int depth = 0;

        while (!q.isEmpty()){
            int size = q.size();
            while (size-- > 0){
                TreeNode node = q.poll();

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
    def maxDepth(self, root: TreeNode) -> int:
        if not root: return 0
        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right))
    # bfs
    def maxDepth(self, root):
        if not root: return 0
        q = collections.deque([(root, 1)])
        while q:
            curr, depth = q.popleft()
            if not curr.left and not curr.right and not q:
                return depth
            if curr.left:
                q.append((curr.left, depth + 1))
            if curr.right:
                q.append((curr.right, depth + 1))
```

#### 总结

第104题是简单题，只需熟悉二叉树的深度优先搜索与广度优先搜索，并记录对应的层次即可。