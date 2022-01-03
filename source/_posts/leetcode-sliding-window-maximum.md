---
title: LeetCode第239题Sliding Window Maximum
date: 2019-09-28 08:02:09
updated: 
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

#### Sliding Window Maximum

[Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)，给定一个数组`num`，一个大小为`k`的滑动窗口，该窗口从数组的最左边移到最右边，得出窗口内的最大值。

##### Example:

>Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
>Output: [3,3,5,5,6,7] 
>Explanation: 

||||||||| Max|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|__[1__|__3__|__-1]__|-3|5|3|6|7|__3__|
|1|__[3__|__-1__|__-3]__|5|3|6|7|__3__|
|1|3|__[-1__|__-3__|__5]__|3|6|7|__5__|
|1|3|-1|__[-3__|__5__|__3]__|6|7|__5__|
|1|3|-1|-3|__[5__|__3__|__6__]|7|__6__|
|1|3|-1|-3|5|__[3__|__6__|__7]__|__7__|

这一题和[Kth Largest Element in a Stream](https://wqdchn.github.io/leetcode-kth-largest-element-in-a-stream.html)寻找一个整数流中第`K`大的元素非常相似，也可以使用队列来完成。这里使用一个双端队列`Deque`，队列中存放当前窗口最大值的数组下标，每当滑动窗口的时候，就记录一次结果。

#### Solution

下面是`Java`的代码：

```Java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || k <= 0) return new int[0];
        int[] res = new int[nums.length - k + 1];//保存结果
        ArrayDeque<Integer> deque = new ArrayDeque<Integer>();//双端队列

        int index = 0;
        for (int i = 0; i < nums.length; i++){
            while (!deque.isEmpty() && deque.peek() < i - k + 1){//越界
                deque.poll();
            }

            while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]){
                deque.pollLast();//从右向左剔除，确保最左是当前窗口最大值
            }

            deque.offer(i);//存放的是位置索引

            if (i >= k - 1){
                res[index++] = nums[deque.peek()];
            }
        }
        return res;
    }
}
```

下面是`Python`的代码：

```Python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:    
        if not nums: return []
        window , res = [], []
        for i, x in enumerate(nums):
            if i >= k and window[0] <= i - k:
                window.pop(0)
            while window and nums[window[-1]] <= x:
                window.pop()
            window.append(i)
            if i >= k -1:
                res.append(nums[window[0]])
        return res
```

#### 总结

第239题是难题，但是使用合适的数据结构和对应的逻辑，就能很好的完成，自带API真香。