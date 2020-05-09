---
title: c++ stl
date: 2020-04-20 17:10:24
tags:
	- c++
	- stl
---

![image-20200430092149247](https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200430092149247.png)

<!-- more -->

# 什么是STL

stl: standard template library 标准模板库

由三个部分组成：

* container：存储数据
* iterator：访问数据和移动数据
* algorithm：提供了一些通用的算法

不需要去自己create模板库，但是自己要学会使用





# 内容

> 参考链接：https://www.youtube.com/watch?v=OnnRobBeaLc&list=PLk6CEY9XxSIA-xo3HRYC3M0Aitzdut7AA&index=2

## std::array

* 头文件`#include <array>`
* 语法`std::array<T, N>`
* notice：
  * std::array是一个容器，里面封装了固定数组
* 访问元素
  * at( )
  * [ ]
  * front()
  * back()
  * data() 



## std::vector

* 头文件`#include <vector>`
* 语法`std::vector<T>`
* 动态数组，长度可以随便改变