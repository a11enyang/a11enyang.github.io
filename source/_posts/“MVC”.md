---
title: MVC
date: 2020-04-26 19:37:29
tags:
	- MVC
---

![image-20200430090840049](https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200430090840049.png)

<!-- more -->


```txt
Presentation Layer  表现层
  -> Views
  -> ViewModels  
  -> Controllers 

Service Layer   服务层（中心业务逻辑）
  -> Includes business logic
  -> Uses data access interfaces

Data Access Layer (DAOs)  数据访问层（抽象了数据存储机制）
  -> Contracts (interfaces) for persistent storage
  -> Interface implementations

Entities （实体类）
  -> POCO/ POJO that represent data 
```

POCO是指Plain Old Class Object，也就是最基本的CLR Class，在原先的EF中，实体类通常是从一个基类继承下来的，而且带有大量的属性描述。而POCO则是指最原始的Class，换句话说这个实体的 Class仅仅需要从Object继承即可，不需要从某一个特定的基类继承。主要是配合Code First使用。Cost Frist则是指我们先定义POCO这样的实体class，然后生成数据库。实际上现在也可以使用Entity Framework Power tools将已经存在的数据库反向生成POCO的class(不通过edmx文件)



```架构
View ---> Controller ---> Service ---> Data Access Layer
```







https://blog.csdn.net/zdwzzu2006/article/details/6053006