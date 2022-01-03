---
title: LeetCode第232题Implement Queen using Stacks
date: 2019-09-25 18:41:12
updated: 
toc: true
top: 
categories: 
- 数据结构与算法
tags:
- LeetCode
- 栈
- 队列
---
<!-- more -->

#### Implement Queen using Stacks

[Implement Queen using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)用栈实现一个队列。

##### Example:

>MyQueue queue = new MyQueue();
>queue.push(1);
>queue.push(2);  
>queue.peek();  // returns 1
>queue.pop();   // returns 1
>queue.empty(); // returns false

队列的特性是先进先出，栈的特性是先进后出，我们要实现队列的三个操作，分别是`push`、`pop`、`peek`，还有一个队列状态检查`empty`。可以使用两个栈来实现，分别是`input`、`output`，两者的分工非常清楚：
- 所有的`push`操作，进队列的元素通通丢到`input`栈中。
- 所有的`pop`、`peek`操作，出队列的元素通通在`output`栈中输出。
- 状态检查时，只需要检查两个栈是否都为空，如果是，则队列为空。

需要注意的地方是，两个栈之间如何互相“倒腾”元素。例如我们`push`了4次得到一个队列`[1，2，3，4]`，然后`pop`一次弹出元素`[1]`，队列中剩下`[2，3，4]`，我们再往里`push`元素`[5]`，得到队列`[2，3，4，5]`。那么这个过程中，`input`栈与`output`栈之间需要“倒腾”一下，4次`push`很简单，元素通通压入了`input`栈，此时`output`栈为空。

|input|#|output|
|:-:|:-:|:-:|
|4|#|null|
|3|#|null|
|2|#|null|
|1|#|null|

当我们执行`pop`时，需要把`input`栈中的元素“倒腾”到`output`栈中，再从`output`栈中`pop`出栈顶元素，此时`input`栈为空。

|input|#|output|
|:-:|:-:|:-:|
|null|#|1|
|null|#|2|
|null|#|3|
|null|#|4|

再往队列中`push`元素`[5]`时，需要把`output`栈中的元素“倒腾”到`input`栈中，再将新元素`[5]`给`push`到栈顶，此时`output`栈为空。

|input|#|output|
|:-:|:-:|:-:|
|5|#|null|
|4|#|null|
|3|#|null|
|2|#|null|

#### Solution

下面是`Java`的代码：

```Java
class MyQueue {
    Stack<Integer> input ;
    Stack<Integer> output ;
    /** Initialize your data structure here. */
    public MyQueue() {
        input = new Stack<>();
        output = new Stack<>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        if (input.isEmpty()){
            while (!output.isEmpty()){
                input.push(output.pop());
            }
        }
        input.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if (output.isEmpty()){
            while (!input.isEmpty()){
                output.push(input.pop());
            }
        }
        return output.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        if (output.isEmpty()){
            while (!input.isEmpty()){
                output.push(input.pop());
            }
        }
        return output.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return input.isEmpty() && output.isEmpty();
    }
}
```

下面是`Python`的代码：

```Python
class MyQueue:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.input = []
        self.output = []

    def push(self, x: int) -> None:
        """
        Push element x to the back of queue.
        """
        if len(self.input) == 0:
            while len(self.output) != 0:
                self.input.append(self.output.pop())

        self.input.append(x)

    def pop(self) -> int:
        """
        Removes the element from in front of queue and returns that element.
        """
        if len(self.output) == 0:
            while len(self.input) != 0:
                self.output.append(self.input.pop())

        return self.output.pop()

    def peek(self) -> int:
        """
        Get the front element.
        """
        if len(self.output) == 0:
            while len(self.input) != 0:
                self.output.append(self.input.pop())

        return self.output[-1]

    def empty(self) -> bool:
        """
        Returns whether the queue is empty.
        """
        return len(self.input) == 0 and len(self.output) == 0
```

#### 总结

第232题是简单题，两个数据结构的特性也非常清楚，关键在于理清先进先出与先进后出两个特性之间的关联。