---
title: LeetCode第20题Valid Parentheses
date: 2019-09-19 22:02:38
updated: 
toc: true
top: 
categories: 
- 数据结构与算法
tags:
- LeetCode
- 栈
---
<!-- more -->
#### Valid Parentheses

[Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)的要求是匹配括号的合法性。

##### Example 1:

>Input: "()"
>Output: true

##### Example 2:

>Input: "()[]{}"
>Output: true

##### Example 3:

>Input: "(]"
>Output: false

~~解法明天再写，回去洗洗睡先。~~

方法是使用一个栈的数据结构来匹配括号，我们将每一个元素和三种括号分别进行匹配，如果是`“(”`那么就往栈里`push`进一个它的“解”也就是`“)”`，中括号和大括号也类似，也就是说我们匹配到一个左括号就往栈里`push`进一个“解”—右括号。

假设当前的括号序列是`“()[]{}”`，当匹配到第一个元素`“(”`的时候，栈里面就`push`进一个`“)”`，当匹配走到第二个元素`“)”`的时候，由于它不是待匹配的左括号，而是“解”，所以我们`pop`栈中的内容，刚好发现栈里面有一个`“)”`,于是将它`pop`出去，同理解决余下的元素，最后发现，栈是空的，且所有元素都匹配完了，说明没有非法的括号。如果中途栈就空了，而当前待匹配的不是左括号而是“解”—右括号，则说明已经无解了。

#### Solution

下面是`Java`的代码：

```Java
class Solution {
    public boolean isValid(String s) {
        if (s == null || s.length() == 0) return true;
        Stack<Character> stack = new Stack<>();
        for (Character ch : s.toCharArray()) {
            if (ch == '('){
                stack.push(')');
            } else if (ch == '['){
                stack.push(']');
            } else if (ch == '{'){
                stack.push('}');
            } else {
                if (stack.isEmpty() || stack.pop() != ch){
                    return false;
                }
            }
        }
        return stack.isEmpty();
    }
}
```

下面是`Python`的代码：

```Python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        paren_map = {')':'(', ']':'[', '}':'{'}
        for c in s:
            if c not in paren_map:
                stack.append(c)
            elif not stack or paren_map[c] != stack.pop():
                return False
        return not stack
```

#### 总结

第20题是简单题，只要对栈这种数据结构稍微熟悉就能知道该怎么办，数据结构与算法的课程讲到栈这一节的时候也提到过括号匹配的问题~~我猜大部分同学用的是严蔚民老师的那本经典教材~~。