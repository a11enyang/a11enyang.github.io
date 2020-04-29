---
title: Google file sytem
date: 2020-04-20 00:03:54
tags:
	- GFS
	- Hadoop
	- 大数据
---

<iframe width="966" height="543" src="https://www.youtube.com/embed/WLad7CCexo8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<!-- more -->

## GFS简介

> 参考链接：https://www.youtube.com/watch?v=WLad7CCexo8

#### 1.如何保存一个文件

文件分为许多小块进行存储。在单机上，这些小块都是连续的地址；在集群上，这些小块是分布的，并不是连续的。通常一个文件需要包含文件信息，和索引。

![image-20200417083930033](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img/image-20200417083930033.png)


![image-20200417084143772](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img/image-20200417084143772.png)



当需要保存超大文件的时候，需要一个Master结点来保存文件的相关信息和索引

![image-20200417084339998](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img/image-20200417084339998.png)

#### 2.减少Master机器的数据和流量

Hadoop的设计思想就是：在Mater机器上设置NameNode，这个节点保存文件块的位置（文件的哪一块在哪台机器上）；在Slave机器上设置DataNode节点，这个节点保存文件块的偏移量，所以在文件块的顺序发生改变的时候是不需要通知MasterNode，这样减少了数据和交互。

![image-20200417084851059](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img/image-20200417084851059.png)

#### 3.如何发现数据损坏

通过对文件进行哈希来判断是否损坏

![image-20200417090053397](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img/image-20200417090053397.png)

#### 4.减少ChunkServer挂掉带来的损失

![image-20200417090400227](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img/image-20200417090400227.png)

3和5是在同一地点的不同机架上，4是在不同的地点。

![image-20200417090530473](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img/image-20200417090530473.png)

#### 5.如何发现ChunkServer挂掉

![image-20200417090630796](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img/image-20200417090630796.png)

#### 6.恢复数据

![image-20200417090744753](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img/image-20200417090744753.png)

#### 7.读数据的过程

![image-20200417091017797](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img2/1.png)

#### 8.写数据的过程

![image-20200417091109111](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img2/image-20200417091109111.png)