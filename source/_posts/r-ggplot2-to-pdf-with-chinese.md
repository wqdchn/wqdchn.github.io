---
title: R语言ggplot2绘图转PDF中文乱码问题
date: 2019-10-20 20:53:49
updated: 
toc: true
top: 
categories: 
- R
tags:
- R
- ggplot2
- RStudio
---
<!-- more -->
#### ggplot2与中文乱码的Solution

##### 字体文件

首先假设你有一个`Windows`系统，其次进行R语言编程所使用的`IDE`是`RStudio`~~非常好用的集成开发环境~~。

然后在`Windows`系统中找到中文字体，比如宋体或黑体，再找到一个英文字体，比如`Times New Roman`。它们大概在`C盘`的`Windows/Fonts`路径下。

找出所需的字体文件，比如宋体的字体文件是`simsun.ttc`，黑体的字体文件是`simhei.ttf`，`Times New Roman`的字体文件是`times.ttf`。

找到字体文件就是成功的一半了，然后我把它们随便放到一个地方准备加载。

##### showtext包

然后需要在`R`中调用`showtext`包，如果没有这个包，请安装它。

```R
# 调用showtext包
library(showtext)
showtext_auto(enable = TRUE)

# 载入黑体
font_add("heiti", regular = "F:\\font\\simhei.ttf")

# 载入宋体
font_add("songti", regular = "F:\\font\\simsun.ttc")

# 载入Times New Roman字体
font_add("newrom", regular = "F:\\font\\times.ttf")
```

在上面，我们载入了三种字体，并重命名~~请随意命名~~后供`ggplot2`调用。

注意载入的路径，在`Windows`下好像要使用反斜杠。

注意，使用字体的过程中，可能会出现提示没有这种字体！！！！！！无妨，重新执行一下上面的命令可以解决。

##### ggplot2绘图带中文

```R
p <- ggplot() + geom_point() + ... +
        theme(axis.title.x = element_text(family="newrom"), axis.title.y = element_text(family = "songti")) +
        xlab("Hello World") + 
        ylab("你好世界")
```

在上面的命令中，我们把`X`轴的字体指定为`Times New Roman`，`Y`轴的字体指定为宋体。


##### 转换为PDF

```R
p <- ggplot() + geom_point() + ... +
        theme(axis.title.x = element_text(family="newrom"), axis.title.y = element_text(family = "songti")) +
        xlab("Hello World") + 
        ylab("你好世界")

ggsave("Fig_p.pdf", plot = p)
```
在上面的命令中，我们把`p`转换为了`PDF`。再次提醒，这个过程中，可能会出现提示没有这种字体！！！！！！无妨，重新执行一下上面的字体加载命令即可解决。

然后我们查看一下生成的`PDF`，应该会得到一个`X`轴字体是`Times New Roman`的英文`Hello World`，`Y`轴字体是宋体的中文你好世界。

如果你使用`RStudio`的图片预览功能，并且尝试`Save as PDF`，那么可能依然会出现中文乱码问题。

去试试看吧！

#### 提醒

上面的内容只能解决文字中不包含中英文混合的情况。如果需要中英文混合，那么可能需要把某两种中英文字体文件合并生成新的字体文件，然后加载进来才行。