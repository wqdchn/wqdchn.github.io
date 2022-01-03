---
title: LeetCode第15题Three Sum
date: 2019-09-18 19:16:49
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
#### Three Sum

[Three Sum](https://leetcode.com/problems/3sum/)和[TwoSum](https://leetcode.com/problems/two-sum/)属于类似的题目，`ThreeSum`要求是，给定一个数组和一个目标值，求得数组中`a`+`b`+`c`=`0`的三个数`a`，`b`，`c`，这里目标数定为了零。

##### Example:

> Given array nums = [-1, 0, 1, 2, -1, -4]

> A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]

解法如下，首先对数组进行从小到大排序，假定当前这个数`nums[i]`为`a`，我们在后面的数中寻找`b`和`c`，使得`a`+`b`+`c`=`0`。记住，我们已经排序过了，所以可以从两头进行夹逼，遍历满足条件的`b`和`c`，当`a`+`b`+`c`>`0`的时候，剩余数的末尾`nums[length-1]`向左走，当`a`+`b`+`c`<`0`的时候，剩余数的开头`nums[i+1]`向右走，。

#### Solution

下面是`Java`的代码：

```Java
class Solution {
    public static List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);

        for (int i = 0; i + 2 < nums.length; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {//一次循环过后要么找到一组解，要么无解
                continue;//此处跳过重复解
            }
            int j = i + 1;
            int k = nums.length - 1;
            int target = -nums[i];
            while (j < k) {
                if (nums[j] + nums[k] == target) {
                    res.add(Arrays.asList(nums[i], nums[j], nums[k]));
                    j++;
                    k--;
                    while (j < k && nums[j] == nums[j - 1]) j++;
                    while (j < k && nums[k] == nums[k + 1]) k--;
                } else if (nums[j] + nums[k] > target) {
                    k--;
                } else {
                    j++;
                }
            }
        }
        return res;
    }
}
```

下面是`Python`的代码：

```Python
class Solution:
    def threeSum(self, nums):
        if len(nums) < 0: return []

        nums.sort()
        res = set()

        for i, v in enumerate(nums[:-2]):
            if i >= 1 and v == nums[i-1]:
                continue
            d = {}
            for x in nums[i+1:]:
                if x not in d:
                    d[-v-x] = 1
                else:
                    res.add((v, -v-x, x))
        return list(map(list, res))
```

#### 总结

第15题是中等题，解法依然很挠头，一遍写不出系列，Keep Moving T_T