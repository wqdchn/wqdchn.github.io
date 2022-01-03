---
title: 说一说二分查找
date: 2020-02-20 21:08:46
categories: 
- 数据结构与算法
tags:
- 位运算
- 二分查找
---
<!-- more -->
### 二分查找

二分查找也称折半查找，思想很简单，对于一个有序数组，查找数组中是否存在指定元素，只需比较指定元素与数组中间元素的大小关系，通过这种比较来判断下一次查找的范围，直到完成整个查找。尽管二分查找的基本思想相对简单，但有时它的细节可以令人难以招架。

### 一个简单的二分查找实现

```Java
 public int binSearch(int[] nums, int key) {
    int left = 0;
    int right = nums.length - 1;

    while (left <= right) {
      int mid = left + ((right - left) >>> 1);
      if (nums[mid] > key) {
        right = mid - 1;
      } else if (nums[mid] < key) {
        left = mid + 1;
      } else {
        return mid;
      }
    }
    return -1;
  }
```
