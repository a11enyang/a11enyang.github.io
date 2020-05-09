---
title: springboot学习
date: 2020-04-25 20:32:23
tags:
	- springboot
---

![image-20200430093935388](https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200430093935388.png)

<!-- more -->

### 层次

三个层次：mvc的架构

m：model  实体类  model中一般有两种类，一种是实体类（用于创建数据库实体），一种是数据库类，用于执行数据库操作

v：view 视图 html页面 

c：controller 处理浏览器发送来的请求

service: 处理中心业务逻辑

一般流程

model

* 创建实体类
* 创建数据库接口，继承JpaRespository<Object, ID type>



service

* 创建service类
* 调用数据库接口的自定义方法或者继承的方法



controller

* 创建controller类进行处理浏览器发送来的请求



<img src="/Users/austin/Library/Application Support/typora-user-images/image-20200426095049246.png" alt="image-20200426095049246" style="zoom:50%;" />





#### 常用注解

@RestController 将类注册为Controller类来实现业务逻辑, 返回值是什么就返回什么

@RequestMapping("  ") url映射   可以放在类前面或者是方法的前面

@Controller  返回一个字符串，但是这个字符串是映射了一个模板,spring会直接在模板文件夹里面进行寻找

//这两个一般都是放在Controller类之前的



@GetMapping("  ") 在Controller类的内部的方法的前面，是在RequestMapping的基础上进行路由的

* @GetMapping("/") 正常情况
* @GetMapping("/{id}") 如果是这种情况，在方法的参数的前面需要加上@PathVariable来实现效果, 参数需要保持一致

或者是通过传参的方式，那么就不需要保持一直@PathVariable("p")



@PostMapping( "")



@RequesParam 接受的是body传过来的参数



@Value("${book.name}") 将配置问价中的值在Controller中进行使用



@ConfigurationProperties(prefix = "book")



@Component 组件



@Autowired

> 参考链接： https://juejin.im/post/5ea2593f6fb9a03c73799bf4

### jpa

java持久化api，定义对象关系映射以及实体对象持久化的标准接口，jpa是一套接口规范。

spring data jpa是spring基于orm框架，jpa规范的基础上封装的一套jpa应用框架

常见的jpa数据操作：[https://blog.csdn.net/wangmx1993328/article/details/89603744#JpaRepository%20%E6%8E%A5%E5%8F%A3%E5%B8%B8%E7%94%A8%20API%C2%A0](https://blog.csdn.net/wangmx1993328/article/details/89603744#JpaRepository 接口常用 API )



### 踩坑记录

* 在写实体类的时候记得加上get和set的方法，不然不可以获取数据库中的数据
* 在设置dao层次的时候，记得将id设置正确
* putMapping需要在 form-urlencoded中设置
* postMapping 需要在form-data中设置
* 当需要对数据库中的内容进行修改的时候，需要设置事务
* 根据https://blog.csdn.net/itkool/article/details/82994124说，springboot2已经集成了thymeleaf3，不需要再去可以添加thymeleaf3
* vue中的方法是methods，复数形式





### 学习参考网站

http://www.masterspringboot.com/