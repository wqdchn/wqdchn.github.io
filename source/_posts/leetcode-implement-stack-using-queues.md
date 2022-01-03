---
title: LeetCode第225题Implement Stack using Queues
date: 2019-09-26 09:44:11
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

#### Implement Stack using Queens

[Implement Stack using Queens](https://leetcode.com/problems/implement-stack-using-queues/)用队列实现栈。

##### Example

>MyStack stack = new MyStack();
>stack.push(1);
>stack.push(2);  
>stack.top();   // returns 2
>stack.pop();   // returns 2
>stack.empty(); // returns false

栈的特性是先进后出，队列的特性是先进先出，我们要实现栈的三个操作，分别是`push`、`pop`、`top`，还有一个栈状态检查`empty`。类似用栈实现队列时，使用两个栈互相倒腾，这里用队列实现栈也可以使用两个队列来实现栈，燃鹅本文选择使用一个队列来实现栈，非炫技也，实有趣耳。

使用一个队列时，`push`操作很平常，就把元素压入队列中即可，即压入栈中。

| | | | |
|:-:|:-:|:-:|:-:|
|1|2|3|4|

执行`pop`操作时，由于栈是先进后出的，先前`push`入队的队列头部元素应该在栈底，而队列尾部元素是栈顶。也就是说，我们要`pop`的是队列尾部元素，那么怎么将尾部元素`pop`出去呢？

很简单，我们让除了队列末尾元素之外的其他队列元素出队，那原来的队尾元素不就变成新的队首元素了吗，让这个新的队首元素出队就是栈的`pop`操作，而出队的其他元素则再依次入队，保证栈`pop`之后其他元素没有丢失。

| | | | |
|:-:|:-:|:-:|:-:|
|4|1|2|3|

执行`top`操作时和`pop`类似，只不过我们返回队首元素即可，注意，由于这个队首元素是原来的队尾元素，所以我们在使用过它之后，要让它出队再入队，保证它依然处于队尾，也就是栈顶。

状态检查只需查看队列是否为空即可。

#### Solution

下面是`Java`的代码：

```Java
class MyStack {
    Queue<Integer> q;
    
    /** Initialize your data structure here. */
    public MyStack() {
        q = new LinkedList<>();
    }

    /** Push element x onto stack. */
    public void push(int x) {
        q.offer(x);
    }

    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        for (int i = 1; i < q.size(); i++){
            q.offer(q.poll());
        }
        return q.poll();
    }

    /** Get the top element. */
    public int top() {
        for (int i = 1; i < q.size(); i++){
            q.offer(q.poll());
        }
        int top = q.peek();
        q.offer(q.poll());
        return top;
    }

    /** Returns whether the stack is empty. */
    public boolean empty() {
        return q.isEmpty();
    }
}
```

下面是相似的`Java`代码，请关注两者的区别：

```Java
class MyStack {

    Queue<Integer> q;
    
    /** Initialize your data structure here. */
    public MyStack() {
        q = new LinkedList<>();
    }

    /** Push element x onto stack. */
    public void push(int x) {
        queue.add(x);
        for (int i=1; i<queue.size(); i++)
            queue.add(queue.remove());
    }

    /** Removes the element on top of the stack and returns that element. */
    public void pop() {
        queue.remove();
    }

    /** Get the top element. */
    public int top() {
        return queue.peek();
    }

    /** Returns whether the stack is empty. */
    public boolean empty() {
        return queue.isEmpty();
    }
}

```

下面是`Python`的代码：

```Python
class MyStack:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.queue = []

    def push(self, x: int) -> None:
        """
        Push element x onto stack.
        """
        self.queue.append(x)

    def pop(self) -> int:
        """
        Removes the element on top of the stack and returns that element.
        """
        return self.queue.pop()

    def top(self) -> int:
        """
        Get the top element.
        """
        return self.queue[-1]

    def empty(self) -> bool:
        """
        Returns whether the stack is empty.
        """
        return self.queue == []
```

#### 总结

第225题是简单题，用一个队列实现栈其实也不难，关键依然是理解先进先出和先进后出之间的关联。这里`Python`的逻辑和分析中有所区别，主要是`Python`的内置函数太强大了，完全不需要多余的操作。。。