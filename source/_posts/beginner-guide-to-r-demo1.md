---
title: R语言新手教程：我在哪，我的数据在哪
date: 2020-03-07 18:17:55
updated: 
toc: true
top: 
categories: 
- R
tags:
- R
- RStudio
---

<!-- more -->

#### R语言与RStudio

我知道新手那种对未知的知识，陌生的工具的恐惧和焦虑，一些对于老手来说就是几条命令行，几条代码的事，对于新手来说可能要摸索好几天才能找到头绪。虽说都有这么一个过程，但是能够尽快走过这个阶段总是好的。

今天我在一个`R`语言的群里看到一个新手同学为一个很简单的问题而困恼，不禁想起了我刚接触`R`语言的时候。不过我本身是计算机专业的学生，可能比较容易学习新的工具。但是对很多非计算机专业的学生来说，学习`R`语言可能是一件不那么容易的事。

`R`语言自带了一个`GUI`，也就是编程时的用户界面了，不过它的风格非常朴素，功能也很朴素，不太适合作为新手上手的工具。因此，我强烈建议使用`RStudio`，这里并不介绍`RStudio`的安装、启动，以及`RStudio`面板上各个窗口表示的内容。相信知乎、简书等网站上有很多详细的中文教程。

那么这篇文章要介绍什么东西呢？

很简单，就是如何在`RStudio`命令窗口中使用`R`语言，命令它告诉我，我在哪。

#### 我在哪，我的数据在哪

我执行了读取文件的命令，但是却显示错误，请问是怎么回事啊？？？

这是我在群里最容易看到的一个问题。一看新手同学贴出来的`R`代码可能类似这样：

```R
> read_csv("data.csv")
Error: 'data.csv' does not exist in current working directory ('F:/Documents').

```

报错信息提示我们，文件`data.csv`不在工作路径`F:/Documents`下。也就是说，我现在在的这个地方没有这个东西。那么，首先要知道的就是，我现在在哪里。在`R`中，可以通过`getwd()`命令来获取当前所在的工作路径。

```R
> getwd()
[1] "F:/Documents"
```

输出的信息提示我们，我现在处于文件夹`F:/Documents`目录下。然后我又发现，我的`data.csv`文件其实是在文件夹`F:/Documents/test`目录下的。那么我该怎么让`R`读取到它呢？一种方法是改变工作路径，我的数据在哪里，我就要把哪里里当做工作路径。这可以通过`setwd()`命令来实现。注意，我们需要为该命令指定相应的路径参数，否则该命令将报错，因为你没有告诉`R`到底去哪里。

```R
> getwd()
[1] "F:/Documents"
> setwd("F:/Documents/test")
> getwd()
[1] "F:/Documents/test"
```
现在我们来到了文件夹`F:/Documents/test`目录下，这时，最初那条执行错误的命令可以顺利执行了。

```R
> read_csv("data.csv")
Parsed with column specification:
cols(
  .default = col_double(),
  diagnosis = col_character()
)
See spec(...) for full column specifications.
# A tibble: 569 x 32
```
你看，我们读取到了一个`569 x 32`的数据框，数据成功地进入了`R`环境中。这是为什么呢？首先通过如下命令来查看`read_csv()`的参数。

```R
> args(read.csv)
function (file, header = TRUE, sep = ",", quote = "\"", dec = ".", 
    fill = TRUE, comment.char = "", ...) 
NULL
```

可以看到，`read_csv()`命令中第一个参数是文件。最初的时候，我们位于`F:/Documents`工作路径，我们指定的`data.csv`只告诉了文件名，那么`read_csv()`就会到**相对路径**中寻找。因为`read_csv()`并不清楚这个东西到底在哪里，那没办法了，就在工作路径里找找看，有就有，没有拉倒吧，别的地方我是不会去找的。此时，这个相对路径就是工作路径。而我的`data.csv`文件其实是在文件夹`F:/Documents/test`目录下的，那结果当然是找不到了。

这时候我们灵机一动，如果我在`read_csv()`命令中第一个参数中指定`data.csv`的真实位置和文件名呢？我们依然回到最初的地点，将`F:/Documents`设置为工作路径，再告诉`read_csv()`命令`data.csv`的真实位置和文件名。

```R
> setwd("F:/Documents")
> getwd()
[1] "F:/Documents"
> read_csv("F:/Documents/test/data.csv")
Parsed with column specification:
cols(
  .default = col_double(),
  diagnosis = col_character()
)
See spec(...) for full column specifications.
# A tibble: 569 x 32
```

结果顺利地读取到了这个文件。这又是为什么呢？？？这是因为，我们这次告诉`read_csv()`命令的是**绝对路径**：你必须到这个地方找到这个东西，于是`read_csv()`就乖乖去找了。

对于老手来说，可能会觉得这种问题很可笑。但是偏偏就是这些不起眼的问题，常常给新手同学造成很大的烦恼。

我在哪，我的数据在哪，一切的一切都从这里出发，绽放出耀眼的光彩。