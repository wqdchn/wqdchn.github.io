---
title: Java基础：多态
date: 2020-04-10 07:09:22
updated: 
toc: true
top: 
categories: 
- Java
tags:
- Java
- 多态
---

<!-- more -->

### 多态

我们知道，面向对象的基本要素是封装、继承、多态。其中多态也叫动态绑定，是指程序在执行期间，判断所引用的对象的实际类型，根据实际类型来调用其相应的方法。以下面的代码为例，

``` Java
public class PolymorphTest {

  public static void main(String[] args) {
    Animal animal = new Dog("小白", "白色");
    Animal animal2 = new Cat("小橘", "黑色");
    
    animal.eat();
    animal2.eat();
  }
}


class Animal {

  private String name;

  Animal(String name) {
    this.name = name;
  }

  public void eat() {
    System.out.println("动物在吃东西");
  }
}

class Dog extends Animal {

  private String Color; // 肤色

  Dog(String name, String Color) {
    super(name);
    this.Color = Color;
  }

  public void eat() {
    System.out.println("小狗在吃骨头");
  }

}

class Cat extends Animal {

  private String eyeColor; // 眼睛色

  Cat(String name, String eyeColor) {
    super(name);
    this.eyeColor = eyeColor;
  }

  public void eat() {
    System.out.println("小猫在吃小鱼干");
  }
}

输出：

小狗在吃骨头
小猫在吃小鱼干
```

程序在执行 Animal animal = new Dog("小白", "白色") 时，栈空间内会有一个变量 animal ，animal 指向堆空间中 new 出来的 Dog 对象。在 new 这个 Dog 对象的时候，首先会调用父类 Animal 的构造方法，把名字小白传递给构造方法 Animal(String name) ，然后再初始化自己的私有成员变量 Color。这样，当 Dog 对象的构造方法 Dog(String name, String Color) 执行完毕之后，内存空间中就出现了一个 Dog 对象和一个 Dog 对象的引用 animal 变量，这个 Dog 对象有自己的私有成员变量 Color，同时，Dog 对象中还包含了一个父类的 Animal 对象，这个 Animal 对象中包含了 Dog 对象的名字。

在方法区中，则存在两个 eat() 方法，分别是父类 Animal 对象的 eat() 方法和子类 Dog 对象的 eat() 方法，当我们执行 animal 的 eat 方法调用时，多态就发挥作用了，它会根据实际 new 出来的对象去取得对应的方法，并执行，所以我们看见了 小狗在吃骨头 而不是 动物在吃东西。

这里，我们小结一下，多态是指程序在执行期间，判断所引用的对象的实际类型，根据实际类型来调用其相应的方法，实现多态需要三个条件，分别是要有继承，子类要重写 Override 父类的方法，父类引用指向子类对象。

### 多态的实现

那么多态是怎么实现的呢？回想一下类的加载机制，在类加载的准备阶段，Java 虚拟机会为类的静态字段分配内存，同时构造与该类相关联方法表，这个方法表就是实现动态绑定也就是多态的关键所在。

方法表本质是一个数组，它存储了当前类及其父类中非私有的实例方法。在动态绑定过程中，程序会访问栈空间上的调用者，根据其指向的对象取得调用者的实际类型，然后读取该类型的方法表，进而读取方法表中某个索引下对应的目标方法。

### 总结

关于多态更加细致的实现暂时先不讨论了，先了解到这里吧。