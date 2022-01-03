---
title: LeetCode第7题Reverse Integer
date: 2019-09-15 12:09:44
updated: 
toc: true
top: 
categories: 
- 数据结构与算法
tags:
- LeetCode
- 数组
---
<!-- more -->

#### Reverse Integer

[Reverse Integer](https://leetcode.com/problems/reverse-integer/)的题目要求很简单，给定32位有符号整数，返回该整数翻转后的结果，结果中零在第一位的省略零。

##### Example 1:

>Input: 123
>Output: 321

##### Example 2:

>Input: -123
>Output: -321

##### Example 3:

>Input: 120
>Output: 21

同样，考虑一个简单粗暴的方法，将整数转换成字符串，将字符串转换成数组，对数组做逆序操作。当然要先判断这个整数的正负，然后考虑翻转后有没有零。

#### Solution

下面是`Java`的代码：

```Java
Class Solution{
    public static int reverse(int n) {
        if (n > Integer.MAX_VALUE || n < Integer.MIN_VALUE) {
            return 0;
        }
        String s = String.valueOf(n);
        if (n < 0) {
            s = s.substring(1, s.length());
            char[] array = s.toCharArray();
            String reverse = "-";
            for (int i = array.length - 1; i >= 0; i--) {
                reverse += array[i];
            }
            s = reverse;
        } else {
            char[] array = s.toCharArray();
            String reverse = "";
            for (int i = array.length - 1; i >= 0; i--) {
                reverse += array[i];
            }
            while (reverse.length() > 1 && reverse.charAt(0) == '0') {
                reverse = reverse.substring(1, reverse.length());
            }
            s = reverse;
        }
        int i = 0;
        try {
            i = Integer.parseInt(s.toString().trim());
        } catch (Exception e) {
            return 0;
        }
        return i;
    }
}
```

下面是`Python`的代码：
```Python
class Solution:
    def reverse(self, x: int) -> int:
        if x >= 0:
            res = int(str(x)[::-1])
        else:
            res = int('-' + str(x)[:0:-1])
        return res if -2147483648 <= res <= 2147483647 else 0
```

#### 总结

第7题是简单题，`LeetCode`给出的标准解法有点挠头，先不管了~~我好菜啊~~。另外不得不说`Python`的内置函数真的太强大了T_T。