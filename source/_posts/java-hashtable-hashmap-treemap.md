---
title: 对比Hashtable、HashMap、TreeMap有什么不同？
date: 2020-04-01 08:14:00
updated: 
toc: true
top: 
categories: 
- Java
tags:
- Java
- 容器
- 集合
- 哈希表
---

<!-- more -->

### Map容器

Map 通常被包括在 Java 集合框架中，但是其本身并不是真正的 Collection 集合类型。 Hashtable 、 HashMap 、 TreeMap 都是 Map 的实现，是以键值对的形式存储和操作数据的容器类型。

![Java Map](https://raw.githubusercontent.com/wqdchn/blog-image/master/java-hashtable-hashmap-treemap/Java Map.png)

 Hashtable 继承自  Dictionary 类，而 HashMap 与 TreeMap 继承 AbstractMap 类，它们的类结构上是不同的，不同的实现表明了它们不同的设计目的。

 Hashtable 是 Java 类库关于哈希表的一个早期实现，它的方法都使用 synchronized 进行同步，是线程安全的。 Hashtable 不允许 key 为 null ，不允许 value 为 null 。   

 HashMap 是使用最广的一种哈希表实现，大部分方法与 Hashtable 是相似的，但是减少了同步开销，因此是线程不安全的。 HashMap 允许 key 为 null ，允许 value 为 null 。

 TreeMap 是基于红黑树的一种提供顺序访问的 Map ，与前两者不同，它的 put() 、 get() 等操作的时间复杂度都是 O(log(n)) 。其顺序可以通过 Comparator 来决定，或者根据键值的自然顺序 Comparable 来决定。它也是线程不安全的。 TreeMap 不允许 key 为 null ，允许 value 为 null 。

对于 TreeMap ，当实现 Comparator 接口时，若未对 null 情况进行判断，则可能抛 NullPointerException 异常。如果针对 null 情况实现了特别处理，则可以 put() 存入，但是却不能正常使用 get() 访问，只能通过遍历去访问。

因此，建议遵循设计的规范，不要做这种使用错误。例如 HashMap 明确声明是线程不安全的，如果不加考虑，直接简单地应用在多线程场景中，总是要出问题的。

### HashMap的实现

 HashMap 的底层是数组和链表组成的复合结构，数组被分成桶 bucket ，里面存放哈希值，通过哈希值来确定数组寻址时的数组下标。哈希值相同的键值对则以链表的形式存储，如果链表的长度超过阈值 TREEIFY_THRESHOLD = 8 ，则对链表进行改造，转化为红黑树。当红黑树的节点小于阈值 UNTREEIFY_THRESHOLD = 6 时，则对红黑树进行改造，转化为链表。我想这是为了避免链表在阈值附近频繁地转换造成过大的开销而设置的两个临界点。

### HashMap的工作流程

#### 存储对象

存储对象时，将键值对 K/V 传给 put() 方法：

```Java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
``` 

然后调用 hash() 方法计算 K 的哈希值：

```Java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```  

这里进行了一个 hashCode() 高位数据移位到低位 h >>> 16 并进行异或运算 ^ 的操作。将高位和低位进行异或运算，只要有高位或低位中有一位的变化，整个 hash() 返回的哈希值就会发生变化，尽可能地减少哈希碰撞。

在计算得到数组下标之后，通过 putVal() 方法进行存储， putVal() 方法本身的逻辑非常密集，从初始化、扩容、树化都与它有关：

```Java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ++modCount;
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    return null;
}
```

如果哈希表为 null ， resize() 方法会负责初始化 tab = resize() 。

当哈希表容量不足时，出现 ++size > threshold ， resize() 方法还会进行扩容。默认的初始化容量参数和最大容量参数如下。

```Java
/**
    * The default initial capacity - MUST be a power of two.
    */
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16， 2的4次方
static final int MAXIMUM_CAPACITY = 1 << 30; // 2的30次方
``` 

如果 K 的 hash 值在 HashMap 中不存在，则执行插入，若存在，则发生碰撞。

如果 K 的 hash 值在 HashMap 中存在，且它们两者 equals 返回 true ，则更新键值对。

如果 K 的 hash 值在 HashMap 中存在，且它们两者 equals 返回 false，则插入链表的尾部（尾插法）或者红黑树中（树的添加方式）。

#### 获取对象

获取对象时，将 K 传给 get() 方法：

调用 hash(K) 方法，计算 K 的 hash 值，从而获取该键值所在链表的数组下标。

```Java
public V get(Object key) {
    Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}
```

然后顺序遍历链表，根据equals()方法查找相同 Node 链表中 K 值对应的 V 值。

```Java
final Node<K,V> getNode(int hash, Object key) {
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (first = tab[(n - 1) & hash]) != null) {
        if (first.hash == hash && // always check first node
            ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        if ((e = first.next) != null) {
            if (first instanceof TreeNode)
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            do {
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}
```

#### 扩容

扩容的方法比较复杂

```Java

static final float DEFAULT_LOAD_FACTOR = 0.75f; // 负载因子

final Node<K,V>[] resize() {
    // ...
    else if ((newCap = oldCap << 1) < MAXIMUM_CAPACIY &&
                oldCap >= DEFAULT_INITIAL_CAPAITY)
        newThr = oldThr << 1; // double there
       // ... 
    else if (oldThr > 0) // initial capacity was placed in threshold
        newCap = oldThr;
    else {  
        // zero initial threshold signifies using defaultsfults
        newCap = DEFAULT_INITIAL_CAPAITY;
        newThr = (int)(DEFAULT_LOAD_ATOR* DEFAULT_INITIAL_CAPACITY；
    }
    if (newThr ==0) {
        float ft = (float)newCap * loadFator;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?(int)ft : Integer.MAX_VALUE);
    }
    threshold = neThr;
    Node<K,V>[] newTab = (Node<K,V>[])new Node[newap];
    table = n；
    // 移动到新的数组结构e数组结构 
}

```

如果负载因子 * 容量 > 元素数量，则会进行扩容。扩容时创建一个新的数组，其容量为旧数组的两倍，并重新计算旧数组中结点的存储位置。结点在新数组中的位置只有两种，原下标位置或原下标+旧数组的大小。

#### 树化改造

在使用 put() 方法进行存储对象时，除了会遇到扩容方法，还有可能遇到树化改造：

```Java

static final int MIN_TREEIFY_CAPACITY = 64;

final void treeifyBin(Node<K,V>[] tab, int hash) {
    int n, index; Node<K,V> e;
    if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
        resize();
    else if ((e = tab[index = (n - 1) & hash]) != null) {
        TreeNode<K,V> hd = null, tl = null;
        do {
            TreeNode<K,V> p = replacementTreeNode(e, null);
            if (tl == null)
                hd = p;
            else {
                p.prev = tl;
                tl.next = p;
            }
            tl = p;
        } while ((e = e.next) != null);
        if ((tab[index] = hd) != null)
            hd.treeify(tab);
    }
}
```

如果容量小于 MIN_TREEIFY_CAPACITY，只会进行简单的扩容。

如果容量大于 MIN_TREEIFY_CAPACITY ，则会进行树化改造。

HashMap 之所以要进行树化改造，是因为底层结构中链表的遍历时间复杂度是 O(N) 的，在哈希碰撞较为严重的时候，链表较长，其存取性能较低，还可能有额外的安全隐患。

### hashCode

hashCode 是所有 Java 对象的固有方法，如果不重载的话，返回的实际上是该对象在 JVM 的堆上内存地址，而不同对象的内存地址肯定不同，所以这个 hashCode 也就肯定不同了。如果重载了的话，由于采用的算法的问题，有可能导致两个不同对象的hashCode相同。

### equals 的特性。

自反性：对于任何非空引用值 x，x.equals(x) 都应返回 true。
对称性：对于任何非空引用值 x 和 y，当且仅当 y.equals(x) 返回 true 时，x.equals(y) 才应返回 true。
传递性：对于任何非空引用值 x、y 和 z，如果 x.equals(y) 返回 true，并且 y.equals(z) 返回 true，那么 x.equals(z) 应返回 true。
一致性：对于任何非空引用值 x 和 y，多次调用 x.equals(y) 始终返回 true 或始终返回 false，前提是对象上 equals 比较中所用的信息没有被修改。
非空性：对于任何非空引用值 x，x.equals(null) 都应返回 false。

### 总结

大部分使用 Map 的场景，通常就是放入、访问或者删除，而对顺序没有特别要求，HashMap 在这种情况下基本是最好的选择。HashMap 的性能表现非常依赖于哈希码的有效性，请务必遵守 hashCode 和 equals 的一些基本约定：

- equals 相等，hashCode 一定要相等。
- 重写了 hashCode 也要重写 equals。
- hashCode 需要保持一致性，状态改变返回的哈希值仍然要一致。

以上内容都基于 jdk1.8.0_161 。

### 参考资料

[为啥 HashMap 的默认容量是16？](https://mp.weixin.qq.com/s?__biz=MzI3NzE0NjcwMg==&mid=2650125459&idx=1&sn=56a14e497b5644eba72f034ac791a4d9&chksm=f36ba9b2c41c20a43611ef0d54747abdaf12f1f867ab23f2f0fc8eac87c657353c734f926cbb&scene=21#wechat_redirect)
[我说我了解集合类，面试官竟然问我为啥 HashMap 的负载因子不设置成1！？](https://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&mid=2651486554&idx=1&sn=23a0786a8c401c799042ac7a9aa0e811&chksm=bd2515258a529c3312285c2536a92a7eb56805ded2eb726b72ab06beabe8e167d00d20789333&scene=126&sessionid=1585703424&key=e7b22394d9386bca990226e723bf6055c836e7d8b7dabe02034009cb225b77c844a21b5027ec82bfa738aa258fea0d41ca89603c0b8e93de1540103a2ec25a970c5c796cb2d499aaa6fce20c5e2772c3&ascene=1&uin=MTIzMjgzNDMyNw%3D%3D&devicetype=Windows+10&version=62080079&lang=zh_CN&exportkey=AxuN6zS95LSdyZL9KQG5Oj0%3D&pass_ticket=LGstMxzdbnh6CyV6rurD3W6ylR1d9GRkDMmoMP80xAWiS91bl%2B2xlMkQ2wePQ23q)