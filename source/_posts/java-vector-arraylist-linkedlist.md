---
title: 对比Vector、ArrayList、LinkedList有何区别？
date: 2020-03-31 08:24:58
updated: 
toc: true
top: 
categories: 
- Java
tags:
- Java
- 容器
- 集合
- 数组
- 链表
---

<!-- more -->

### Java的集合框架

 Java 提供的主要容器类型分别是集合框架与 Map ，集合通常是指 Collection 接口下的 List 、 Set 、 Queue 三类集合。一般认为 Collection 接口是 Java 集合框架的根，其子集构成了集合框架。 Map 并不属于 Collection 接口的子集，但是在概念是也当做集合来使用，但是它本身并不是真正的集合。注意，所有集合中都不能存放基础数据类型，只能存放对象的引用。

![Java Collection](https://raw.githubusercontent.com/wqdchn/blog-image/master/java-vector-arraylist-linkedlist/Java Collection.png)


### List集合

 Vector 、 ArrayList 、 LinkedList 是 List 的实现，都是有序集合。它们都提供了诸如随机访问、添加、删除等常用操作，以及迭代器遍历、排序等方法，在功能上较为相近。但是其具体的设计存在一定区别，因此在性能、线程安全等方面有所不同。

常用方法：

- size() 集合元素个数
- add()/addAll() 添加元素
- remove()/removeAll() 删除元素
- get() 获取元素
- set() 修改元素
- sort() 集合元素排序
- toArray() 转换
- clear() 清空集合

### Vector

 Vector 是 Java 早期提供的线程安全的动态数组，内部使用对象数组类保存数据，其线程安全通过 synchronized 实现。

```Java
public synchronized boolean add(E e) {
    modCount++;
    ensureCapacityHelper(elementCount + 1);
    elementData[elementCount++] = e;
    return true;
}

public synchronized E remove(int index) {
    modCount++;
    if (index >= elementCount)
        throw new ArrayIndexOutOfBoundsException(index);
    E oldValue = elementData(index);

    int numMoved = elementCount - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,
                            numMoved);
    elementData[--elementCount] = null; // Let gc do its work

    return oldValue;
}
```

 Vector 默认创建一个大小为 10 的 Object 数组，并将动态扩展大小 capacityIncrement 设置为 0 。 Vector 能够根据需要进行自动扩容，当数组已满时，会创建新的数组，并拷贝原有数据。在初始化时若指定了容量的动态扩展大小 capacityIncrement > 0 则依据指定的大小进行扩容，否则默认扩展一倍的容量。

```Java
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                        capacityIncrement : oldCapacity);
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    elementData = Arrays.copyOf(elementData, newCapacity);
}

public Vector() {
    this(10);
}

public Vector(int initialCapacity, int capacityIncrement) {
    super();
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal Capacity: "+
                                            initialCapacity);
    this.elementData = new Object[initialCapacity];
    this.capacityIncrement = capacityIncrement;
}
```
### ArrayList

 ArrayList 是应用最多的动态数组，由于它没有了同步开销，因此性能更加良好。相应的，它不是线程安全的。 ArrayList 也支持动态扩容，但是与 Vector 默认扩容一倍不同， ArrayList 扩容时是增加当前容量的 50% ，其默认容量是 10 。

```Java

private static final int DEFAULT_CAPACITY = 10;

private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```

 ArrayList 在执行插入操作时，若元素个数超过当前数组预定义容量的最大值，数组需要扩容，扩容过程需要调用底层 System.arraycopy() 方法进行大量的数组复制操作。它在删除元素时并不会减少数组的容量，但是如果需要缩小数组容量，可以调用 trimToSize() 方法。在查找元素时要遍历数组，对于非 null 的元素采取 equals 的方式寻找。

### LinkedList

 LinkedList 是双向链表，它不需要进行调整容量，它也不是线程安全的。 LinkedList 在插入元素时，须创建一个新的 Entry 对象，并更新相应元素的前后元素的引用；在查找元素时，需遍历链表；在删除元素时，要遍历链表，找到要删除的元素，然后从链表上将此元素删除即可。

### 排序操作

 Vector 、 ArrayList 、 LinkedList 内部都实现了排序操作，允许进行自定义排序。

```Java
public class Test {

  public static void main(String[] args) {
    LinkedList<String> linkedList = new LinkedList<>();

    linkedList.add("c");
    linkedList.add("b");
    linkedList.add("a");

    linkedList.sort(new Comparator<String>() {
      @Override
      public int compare(String o1, String o2) {
        return o1.compareTo(o2);
      }
    });

    linkedList.forEach((s) -> {
      System.out.print(s + " ");
    });

  }
}

输出 a b c 
```


### 总结

 Vector 、 ArrayList 、 LinkedList 是 List 的实现，都是有序集合。 Vector 、 ArrayList 使用数组实现，有需要时可以进行动态扩容， LinkedList 使用双向链表实现，不需要动态扩容。 Vector 是线程安全的，而 ArrayList 、 LinkedList 是线程不安全的。在分析以上三类集合的读写效率时，还应注意的一点是是否需要考尾部的情况。

以上内容都基于 jdk1.8.0_161 。

~~JDK源码是四个空格缩进，而我用的是Google Style两个空格缩进，有点不协调orz。~~

### 参考资料

[Java问答：用ArrayList还是LinkedList](https://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&mid=2651485602&idx=2&sn=569035c3b6bd1a1028df03517418e84a&chksm=bd2519dd8a5290cb32f88a88fc8f57799b5d25e79452bc66749f83c40f593509931506603623&mpshare=1&scene=1&srcid=&sharer_sharetime=1575123269344&sharer_shareid=3c5653a301d37ee49e0cb689e723404f#rd)