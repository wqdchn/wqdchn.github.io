---
title: LeetCode第1题Two Sum
date: 2019-09-12 09:54:13
updated: 
toc: true
top: 
categories: 
- 数据结构与算法
tags:
- LeetCode
- 哈希表
---
<!-- more -->
#### Two Sum

[Two Sum](https://leetcode.com/problems/two-sum/)的题目要求是，给定一个数组和一个目标值，求得数组中两数`num1`和`num2`相加等于目标值`target`的两个数的下标。解法有两种，一是暴力法，一个个比较过来；二是哈希，把数组存在`map`里，存放的过程中只要发现`map`里面存有`num1`对应的目标解`num2`=`target`-`num1`，则返回下标即可。

#### Solution

下面是`Java`的代码：

```Java
class Solution {
    public int[] twoSum(int[] nums,int target){
        int[] result = new int[2];
        for (int i = 0; i < nums.length; i++){
            for (int j = i + 1;j < nums.length; j++){
                if(nums[i] + nums[j] == target){
                    result[0] = i;
                    result[1] = j;
                }
            }
        }
        return result;
    }
}
```

```Java
class Solution {
    public int[] twoSum(int[] nums,int target){
        int[] result = new int[2];
        Map<Integer,Integer> mp = new HashMap<Integer, Integer>();
        for (int i  = 0; i < nums.length; i++){
            if(mp.containsKey(target - nums[i])){
                result[0] = mp.get(target - nums[i]);
                result[1] = i;
                return  result;
            }
            mp.put(nums[i],i);
        }
        return result;
    }
}
```

下面是`Python`的代码：

```Python
class Solution:
    def twoSum(self, nums, target: int):
        mp = {}
        for i, x in enumerate(nums):
            if x in mp:
                return mp[x], i
            else:
                mp[target - x] = i
        return -1
```

#### 总结

第1题是简单题，迈出了第一步，以后也不要停下噢！