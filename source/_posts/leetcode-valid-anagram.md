---
title: LeetCode第242题Valid Anagram
date: 2019-09-29 07:50:14
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
#### Valid Anagram

[Valid Anagram](https://leetcode.com/problems/valid-anagram/)给出两个字符串`s`和`t`，查看字符串`t`是否是字符串`s`的异位词。

##### Example 1:

>Input: s = "anagram", t = "nagaram"
>Output: true

##### Example 2:

>Input: s = "rat", t = "car"
>Output: false

这一题很简单，只需要用哈希的方法来解决，我们使用一个一维数组，数组的长度是26，里面我们存放26个字母的计数，如果字符`c`同时存在与字符串`s`与字符串`t`中，则该字符在数组中`+1`再`-1`记为零，只要数组中存在不为零的元素，则字符串`s`和字符串`t`不是有效的异位词。

还可以使用字符数组排序的方法，如果字符串`s`和字符串`t`是有效异位词，那么两者包含的字符在唯一性上是相等的，排序后的序列也必定相等。

#### Solution

下面是`Java`的代码：

```Java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) return false;

        int[] counter = new int[26];

        for (int i = 0; i < s.length(); i++) {
            counter[s.charAt(i) - 'a']++;
            counter[t.charAt(i) - 'a']--;
        }
        for (int count : counter) {
            if (count != 0) {
                return false;
            }
        }
        return true;
    }

    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        char[] str1 = s.toCharArray();
        char[] str2 = t.toCharArray();
        Arrays.sort(str1);
        Arrays.sort(str2);
        return Arrays.equals(str1, str2);
    }
}
```

下面是`Python`的代码：

```Python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t): return False
        count = collections.defaultdict(int)
        for c in s:
            count[c] += 1
        for c in t:
            count[c] -= 1
            if count[c] < 0:
                return False
        return True
```

#### 总结

第242题是简单题，冲鸭！