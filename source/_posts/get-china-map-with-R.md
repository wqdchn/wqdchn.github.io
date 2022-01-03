---
title: R语言获取标准中国地图
date: 2020-03-04 10:49:48
updated: 
toc: true
top: 
categories: 
- R
tags:
- R
- tmap
- sf
- 地理数据
---
<!-- more -->
### 地图在哪里

空间数据分析离不开基础地理数据的支持，比如省市县乡等各级行政区划的数据。然而我国的基础地理数据处于一种薛定谔的状态，往往找不到一个能给出基础地理数据的政府部门的官网。有些网站会共享一部分基础地理数据，但是申请数据的手续有时非常复杂， 即要注册提交个人信息，又要手持身份证拍照，还要写下申请书。于是在大部分情况下只能转向万能的某宝，说多了都是泪。

最近突然发现，民政部的官网悄咪咪的提供了一个接口可以获取到一些基础地理数据。既然是民政部门提供的数据，那当然是最权威的，再也不用担心有些数据中存在的南海九段线缺失，台湾岛没有等问题啦。

### 获取全国县级地图

民政部提供了省级与县级的 json 格式地图数据，我们可以使用 R 语言的 sf 包读取需要的数据，然后使用 tmap 包来绘制地图。

``` R
library(sf)
library(tmap)

# 民政部官网
url <- "http://xzqh.mca.gov.cn/data/"

# 获取全国县级地图
xian_quanguo <- sf::st_read(
  dsn = paste0(url, "xian_quanguo.json"),
  stringsAsFactors = FALSE
)

# 简单检查一下数据
head(xian_quanguo)

st_crs(xian_quanguo) <- st_crs("+proj=longlat +datum=WGS84")

# tmap绘制地图
tm_shape(xian_quanguo) + 
  tm_polygons() + 
  tm_compass(position = c("left", "bottom")) + # 指北针
  tm_scale_bar(position = c("left", "bottom")) # 比例尺

```

### 总结

sf 包的计算效率非常高，是当前 R 语言空间数据分析过程中不可缺少的重要工具，强烈 push。 tmap 包是我接触的比较友好的一种地图可视化包，使用也很简洁，更新也比较及时。

### 参考资料

[sf: A package that provides simple features access for R](https://github.com/r-spatial/sf)

[tmap: R package for thematic maps](https://github.com/mtennekes/tmap)