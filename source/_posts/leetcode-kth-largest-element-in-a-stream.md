---
title: LeetCode第703题Kth Largest Element in a Stream
date: 2019-09-27 18:26:48
toc: true
top: 
categories: 
- 数据结构与算法
tags:
- LeetCode
- 队列
- 优先队列
---
<!-- more -->

#### Kth Largest Element in a Stream

[Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)寻找一个整数流中第`K`大的元素，`K`是固定的，整数流包含一个构造函数`add`，每次往流中添加新元素时，就返回新的第`K`大的元素。

##### Example:

>int k = 3;
>int[] arr = [4,5,8,2];
>KthLargest kthLargest = new KthLargest(3, arr);
>kthLargest.add(3);   // returns 4
>kthLargest.add(5);   // returns 5
>kthLargest.add(10);  // returns 5
>kthLargest.add(9);   // returns 8
>kthLargest.add(4);   // returns 8

这道题可以使用优先队列`PriorityQueue`来做，在`Java`中优先队列默认通过二叉小顶堆实现。我们可以令这个优先队列的长度为`K`，这个队列的头部就是整数流中第`K`大的元素，每当新的元素进来时，我们就把新的元素和队列头部元素（也就是堆顶）进行比较，如果新的元素比较大，则纳入新的元素。优先队列会自动维护小顶堆的性质，保证队列头部元素始终是我们需要的第`K`大的元素。

#### Solution

下面是`Java`的代码：

```Java
class KthLargest {   
    PriorityQueue<Integer> q;
    int k;

    public KthLargest(int k, int[] nums) {
        this.k = k;
        q = new PriorityQueue<>(k);
        for (int n : nums){
            add(n);
        }       
    }
    
    public int add(int val) {
        if (q.size() < k){
            q.offer(val);
        }else if (q.peek() < val){
            q.poll();
            q.offer(val);
        }
        return q.peek();
    }
}

```

下面是`Python`的代码：


```Python
class KthLargest:
    def __init__(self, k, nums):
        self.pool = nums
        self.k = k
        heapq.heapify(self.pool)
        while len(self.pool) > k:
            heapq.heappop(self.pool)

    def add(self, val):
        if len(self.pool) < self.k:
            heapq.heappush(self.pool, val)
        elif val > self.pool[0]:
            heapq.heapreplace(self.pool, val)
        return self.pool[0]
```

#### 总结

第703题是简单题，学会利用已有的数据结构，加一点简单的逻辑，能做很多神奇的事，再也不要暴力循环了鸭。