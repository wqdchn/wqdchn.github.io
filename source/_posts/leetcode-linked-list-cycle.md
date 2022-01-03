---
title: LeetCode第141题Linked List Cycle
date: 2019-09-25 10:18:10
updated: 
toc: true
top: 
categories: 
- 数据结构与算法
tags:
- LeetCode
- 链表
---
<!-- more -->

#### Linked List Cycle

[Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)判断链表是否有环。

#### Example

>Input: head = [3,2,0,-4], pos = 1
>Output: true
>Explanation: There is a cycle in the linked list, where tail connects to the second node.

![Example](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

方法有两种：
- 一种是使用`Set`的数据结构来遍历整个链表，只要链表中存在环，那么Set中必定会出现重复值，通过这种重复的冲突就可以判断链表中有无环。
- 另一种方法是使用`两个标兵`来遍历，一个跑得比香港记者还要快，一个跑得比香港记者慢，如果链表中存在环，那么快的标兵必定会在跑完一圈之后追上慢的标兵，通过这种追赶也能判断链表中有无环。

#### Solution

下面是`Java`的代码：

```Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */

class Solution {
    public boolean hasCycle(ListNode head) {// Set
        Set<ListNode> nodesSeen = new HashSet<>();
        while (head != null) {
            if (nodesSeen.contains(head)) {
                return true;
            } else {
                nodesSeen.add(head);
            }
            head = head.next;
        }
        return false;
    }
    
    public boolean hasCycle(ListNode head) {// 标兵
        if (head == null || head.next == null) {
            return false;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (slow != fast) {
            if (fast == null || fast.next == null) {
                return false;
            }
            slow = slow.next;
            fast = fast.next.next;
        }
        return true;
    }
}
```

下面是`Python`的代码：

```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head): # Set
        nodeset = set()
        while head:
            if head in nodeset:
                return True
            else:
                nodeset.add(head)
            head = head.next
        return False

    def hasCycle(self, head): # 标兵
        fast = slow = head
        while slow and fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow is fast:
                return True
        return False
```

#### 总结

第141题是简单题，`Set`的方法比较容易想到，`Set`也非常适合做重复检测的工作。