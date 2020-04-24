---
title: Hadoop简介
date: 2020-04-20 09:03:42
tags:
	- Hadoop
	- MapReduce
---

## Hadoop简介 并行计算的框架

#### 前世今生

源自google的三篇论文

![image-20200417091352948](https://raw.githubusercontent.com/a11enyang/Picture/master/img/image-20200417091352948.png)

<!-- more -->

#### 结构

* YARN：负责资源调度
  * ResourceManager：负责资源的协调和调度
  * NodeManager：负责Slave节点的资源申请和情况汇报

*  HDFS：hadoop distributed file system


* MapReduce：基于Yarn大数据计算框架




#### hadoop工作原理简单介绍

> 参考链接：https://knpcode.com/hadoop/overview/what-is-hadoop/

* 将巨型文件复制到HDFS上，框架会将大文件分为很多块，并将这些块分布到集群中的各个节点

  上；

* 然后，编写一个MapReduce程序，该程序具有一些逻辑来处理该数据。您将代码打包为jar，然后将打包的代码传输到存储数据块的DataNodes。这样，您的[MapReduce代码就](https://knpcode.com/hadoop/mapreduce/how-mapreduce-works-in-hadoop/)可以在文件部分上工作（HDFS块位于运行代码的节点上）并并行处理数据。所以要进行的编程作业就是在在这个阶段。

<img src="https://raw.githubusercontent.com/a11enyang/Picture/master/img2/image-20200418180437130.png" alt="image-20200418180437130" style="zoom:50%;" />

#### 架构

* 主从架构：
  * Master：主节点，运行NameNode    ResourceManager等服务进程
  * Slave：从结点，负责DataNode   NodeManager服务进程
* 服务进程举例
  * NameNode,负责记录各个数据块的存储空间(NameSpace),与DataNode进行通信,获取其健康状态信息,并根据決策算法将数据分发到某些节点。
  * DataNode,负责实际数据的存,与NameNode进行通信,将其所在节点的存储状态汇我给NameNode ,以供其决策。

![image-20200417092132279](https://raw.githubusercontent.com/a11enyang/Picture/master/img/image-20200417092132279.png)



> 参考链接：https://www.youtube.com/watch?v=O1QP4GCP3fw&list=PL4B2N39WHsDST4kFzq-rmk3LtRLQxEcto&index=5





#### MapReduce的工作阶段介绍

> 参考链接：https://www.youtube.com/watch?v=CPjSvanPl7s

参考GFS的论文，可以将MapReduce的工作流程分为以下的几个阶段

* imput：将需要分布存储的文档复制到HDFS上（是否需要进行分布式存储文件，处理的文件先是存储在HDFs上的）


* split：将文档分割为块，交给几个不同的机器

* map：每台机器各自处理自己的任务，分解块；数据转换为<key, value>的形式


* shuffle：洗牌，每台机器处理的结果重新整理，不属于自己管理的内容就交给其他机器；重新组合，然后发送到reducer


* reduce：将reducer中内容进行组合


* finalize：最后输出




#### MapReduce工作流程详细介绍

>```txt
>假设文本内容是：
>Today is your birthday
>Hadppy birthday to you
>Today is a nice day
>how is your day
>```

![image-20200420095725741](https://raw.githubusercontent.com/a11enyang/Picture/master/img2/image-20200420095725741.png)

* MapReduce分为两个阶段，Map和Reduce。每个阶段的输入输出内容都是<key, value>形式；

* 在数据进行map task之前，会进行预处理：

  * inputSplit：根据当前的mapper机器的数量n，决定将输入文档分为n部分（比如现在有两台mapper机器，那么文档分为两部分），然后inputSplit会读取当前的mapper的工作内容；

  * RecordReader与inputSplit进行通信，然后进行处理当前部分生成键值对

    ```
    假设当前两台中（以为文本由两台mapper处理）中的一台RecordReader：
    输入：
    Today is your birthday
    Happy birthday to you
    
    输出（键值对）：
    <0, Today is your birthday>
    <23, Hadppy birthday to you>
    key是行开头的偏移量，value是行的内容
    ```

* Mapper

  * 输入接受RecordReader的结果

  * 输出是完整的key,value集合，并且输出内容是不会存储到HDFS上的，输出内容是临时性的，因为这样会造成不必要的麻烦

    ```txt
    <Today, 1>
    <is, 1>
    <your, 1>
    <birthday, 1>
    <Happy, 1>
    <birthday, 1>
    <to, 1>
    <you, 1>
    ```

* Combiner

  * Combiner是局部聚合器，最大限度减少MApper与Reducer之间的数据数据传输

  * 输出

    ```
    <Today, 1>
    <is, 1>
    <your, 1>
    <birthday, 2>
    <Happy, 1>
    <to, 1>
    <you, 1>
    ```

* Partitioner

  * 作为具有相同键值的记录进入同一个分区（每个映射器内）。然后，每个分区被送到一个reducer。分区类决定给定的（key，value）对将进入哪个分区;

    ![image-20200420105757293](https://raw.githubusercontent.com/a11enyang/Picture/master/img2/image-20200420105757293.png)

* shuffle and sort

  shuffle是一个物理移动的过程，将分区中数据移动到reducer上，并且在过程中进行排序，形成list

  ```
  <birthday, 2>
  <day, 2>
  <Today, [1,1]>
  <your, [1,1]>
  ```

* reducer计算最后的结果

  ```
  <birthday, 2>
  <day, 2>
  <Today, 2>
  <your, 2>
  ```

* 最后的输出结果存储在HDFS上



> 在很多教程中，上文中的partitioner是属于shuffle阶段的，本质上没有区别
>
> https://knpcode.com/hadoop/mapreduce/shuffle-phase-in-hadoop-mapreduce/
>
> 其他的理解方式：
>
> <iframe width="966" height="543" src="https://www.youtube.com/embed/6ehu2jEEXWE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

>参考链接：https://techvidvan.com/tutorials/hadoop-mapreduce-key-value-pair/
>
>https://techvidvan.com/tutorials/hadoop-mapreduce-shuffling-and-sorting/
>
>https://techvidvan.com/tutorials/hadoop-partitioner-introduction/
>
>https://techvidvan.com/tutorials/hadoop-combiner-introduction-working-advantages/

