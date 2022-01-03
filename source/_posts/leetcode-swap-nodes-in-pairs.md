---
title: LeetCode第24题Swap Nodes in Pairs
date: 2019-09-22 12:05:28
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
#### Swap Nodes in Pairs

[Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)给定一个链表，交换两个相邻节点，注意，不能修改列表节点中的值，只能更改节点本身的顺序。

##### Example

> Given 1->2->3->4, you should return the list as 2->1->4->3.

链表的结点交换或反转没有什么特别的地方，就是一把梭。`LeeCode`已经给我们定义好了结点`Node`：一个指针，一个结点值，交换函数传入参数的是头指针。当结点数是偶数的时候，没有问题，当结点数是奇数的时候，最后一个结点的下一个结点是`Null`，就不能交换或反转。

#### Solution

下面是`Java`的代码：

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
 
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null) return null;
        if (head.next == null) return head;

        ListNode temp = head.next;
        head.next = head.next.next;

        temp.next = head;
        head = temp;

        head.next.next = swapPairs(head.next.next);

        return head;       
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
    def swapPairs(self, head: ListNode) -> ListNode:
        pre, pre.next = self, head
        while pre.next and pre.next.next:
            a = pre.next
            b = a.next
            pre.next, b.next, a.next = b, a, b.next
            pre = a
        return  self.next
```

#### 总结

第24题是中等题，逻辑很简单，但是编码上有一些细节，别把自己绕晕了。