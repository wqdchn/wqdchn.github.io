---
title: LeetCode第22题Generate Parentheses
date: 2019-09-21 12:15:25
updated: 
toc: true
top: 
categories: 
- 数据结构与算法
tags:
- LeetCode
- 深度优先搜索
---
<!-- more -->
#### Generate Parentheses

[Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)给定一个正整数，生成有效括号的所有组合，这里只需要生成小括号即可。

##### Example

>Input: n = 3
>Output: [
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]

可见，当`n`确定时，生成的解的长度是固定的`2n`，并且解的开头一定是左括号，如果是右括号则是无效的解，左括号与右括号的数量必定是一样的，都是`n`个，这些是分析题目要求得到的“先验知识”了，接下来开始`Code`~~贝叶斯推断bushi~~。

怎么办呢，可以用递归或者说深度优先搜索的方法来遍历，每一个解看成一个长度`2n`的一维数组，我们往里面填充左括号和右括号，起始值都是`n`个，用掉一个就减去一个。如果当前还有左括号，就使用一个左括号，同时，如果右括号的数量比左括号多，则为左括号匹配一个右括号，直到两者都用完为止。

##### Solution

下面是`Java`的代码：

```Java
class Solution {
  public List<String> generateParenthesis(int n) {
    List<String> res = new ArrayList<>();
    generate("", res, n, n);
    return res;
  }

  public void generate(String sublist, List<String> res, int left, int right) {
    if (left == 0 && right == 0) {
      res.add(sublist);
      return;
    }
    if (left > 0) {
      generate(sublist + "(", res, left - 1, right);
    }
    if (right > left) {
      generate(sublist + ")", res, left, right - 1);
    }
  }
}
```

下面是`Python`的代码：

```Python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        def generate(p, left, right, parens=[]):
            if left:
                generate(p + '(', left - 1, right)
            if right > left:
                generate(p + ')', left, right - 1)
            if not right:
                parens += p,
            return parens
        return generate('', n, n)
```

#### 总结

第22题是一个中等题，难度还不算非常大，递归的方法也非常简洁易懂，加油↖(^ω^)↗