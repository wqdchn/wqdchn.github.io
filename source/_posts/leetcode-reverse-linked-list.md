---
title: LeetCode第206题Reverse Linked List
date: 2019-09-24 11:49:05
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
#### Reverse Linked List
[Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)反转单链表。

##### Example:

>Input: 1->2->3->4->5->NULL
>Output: 5->4->3->2->1->NULL

我们需要三个指针，`prev`->`curr`->`next`，从头指针开始反转，令`curr`->`prev`，完成一个元素的反转之后，令其下一个元素为`curr`指针指向的对象，直到当前元素`curr`为空，可以使用遍历或递归的方式来实现。

|prev|curr|next|...| | | | |
|:----:|:-:|:-:|:---:|:-:|:-:|:-:|:-:|
|null|1|2|...|3|4|5|null|
|1|2|3|...|4|5|null|
|2|3|4|...|5|null|
|3|4|5|...|null|
|4|5|null|...|

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
    public ListNode reverseList(ListNode head) {// 遍历
        ListNode re_head = null;
        for (ListNode curr = head; curr != null; ){
            ListNode temp = curr.next;
            curr.next = re_head;
            re_head = curr;
            curr = temp;
        }
        return re_head;
    }

    public ListNode reverseList(ListNode head) {// 递归
        if (head == null || head.next == null) return head;
        ListNode re_head = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return re_head;
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
    def reverseList(self, head):  # 遍历
        re_head, curr = None, head
        while curr:
            curr.next, re_head, curr = re_head, curr, curr.next
        return re_head

    def reverseList(self, head: ListNode) -> ListNode: # 递归
        if head == None or head.next == None:
            return head

        temp = head.next
        re_head = self.reverseList(temp)
        head.next = None
        temp.next = head
        return re_head
```

#### 总结

`LeetCode`题目的难度是变动的，这一题之前还是中等题，最近变成了简单题，可能是练习的人多了，`Accepted`高了，说明基础还是很重要的。