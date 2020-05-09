---
title: springboot教程
date: 2020-05-01 16:42:19
tags:
	- springboot
---

![](https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/Snipaste_2020-05-01_18-57-24.png)

<!-- more -->

项目的github地址：https://github.com/a11enyang/simpleSpringBootDemo

## 创建项目

在idea中选择springboot项目进行创建，最开始依赖只选择web（新版的idea中显示的spring web）



## 查看项目结构

<img src="https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200501170132454.png" alt="image-20200501170132454" style="zoom:50%;" />

## 设置项目结构

在启动类当前的包中添加新的几个包

* model : 存放实体类，springboot会根据这个自动进行orm映射产生数据库表
* dao: 数据库接口，是对数据库的抽象
* service：处理中心业务逻辑

* controller: 处理浏览器发来的请求



![image-20200501170255560](https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200501170255560.png)



## 添加依赖

在新建的springboot项目中，找到`pom.xml`文件，该文件是为了安装依赖

```xml
<!--      使用thymeleaf3    -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
<!--       使用thymeleaf3 end  -->

<!--        bootstrap begin-->
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>bootstrap</artifactId>
            <version>4.4.1</version>
        </dependency>
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>webjars-locator</artifactId>
            <version>0.38</version>
        </dependency>
<!--        bootstrap end-->

<!--        vue begin-->
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>vue</artifactId>
            <version>2.6.11</version>
        </dependency>
        <dependency>
            <groupId>org.webjars.npm</groupId>
            <artifactId>axios</artifactId>
            <version>0.19.0</version>
        </dependency>
<!--        vue end-->
<!--        jpa begin-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
<!--        jpa end-->
        
<!--         mysql begin-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
<!--        mysql end-->
```

上述依赖是本次项目需要使用的内容，请添加到项目中，然后再利用maven进行更新。启动项目，在idea的终端中查看项目是否启动成功。



## 自定义设置

在application.properties中设置数据库连接和thymeleaf。你需要在自己的数据库中建立一个数据库，我的数据库名称是info, 所以下面的设置也是info

```properties
# 数据库设置
spring.datasource.driver-class-name= com.mysql.jdbc.Driver
spring.datasource.url= jdbc:mysql://localhost:3306/info?useUnicode=true&characterEncoding=utf-8
spring.datasource.username=root
spring.datasource.password=ywdssazty5zcy
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

#thymeleaf设置
spring.thymeleaf.mode=HTML


```





## 启动项目

点击idea的项目启动按钮，访问http://localhost:8080/，看是否返回如下页面，如果是，则启动成功。

![image-20200501170824107](https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200501170824107.png)



## 添加实体类

> springboot的注解内容可以查看https://juejin.im/post/5ea2593f6fb9a03c73799bf4

在model包中新建一个Person类

<img src="https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200501172120744.png" alt="image-20200501172120744" style="zoom:50%;" />

```java
package com.example_yangweidong.demo.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    public int id;

    public String name;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```



## 添加dao（data access object 发音“刀”）层接口执行对数据库的操作

<img src="https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200501172451801.png" alt="image-20200501172451801" style="zoom:50%;" />



```
package com.example_yangweidong.demo.dao;

import com.example_yangweidong.demo.model.Person;
import org.springframework.data.jpa.repository.JpaRepository;

public interface PersonRepository  extends JpaRepository<Person, Integer> {
    
}
```

暂时还不需要自己写查询方法，jpa会自动提供一些方法





## 添加PersonService类

<img src="https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200501172925940.png" alt="image-20200501172925940" style="zoom:50%;" />

```java
package com.example_yangweidong.demo.service;

import com.example_yangweidong.demo.dao.PersonRepository;
import com.example_yangweidong.demo.model.Person;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class PersonService {
    /**
     * 自动注入PersonRepository对象，不用自己去new一个
     */
    @Autowired
    public PersonRepository personRepository;

    /**
     * 查找所有person
     * @return
     */
    public List<Person> findAll() {
        return personRepository.findAll();
    }

}

```



## 新建controller类

<img src="https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200501173328845.png" alt="image-20200501173328845" style="zoom:50%;" />

```java
package com.example_yangweidong.demo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/person")
public class PersonController {

    /**
     * 返回template中的thymeleaf页面
     * @return
     */
    @GetMapping("/index")
    public String getIndex() {
        return "index";
    }
}
```



## 在template中新建一个index.html页面

![image-20200501173440020](https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200501173440020.png)

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org" xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <link href='/webjars/bootstrap/4.4.1/css/bootstrap.min.css' rel='stylesheet'>

    <meta charset="UTF-8">
    <title>Home</title>
</head>
<body>

<div class="container" id="main">
    <table class="table table-striped table-bordered">
        <thead>
        <tr>
            <th>ID</th>
            <th>Name</th>

        </tr>
        </thead>
        <tbody>
        <tr v-for="person in persons">
            <td> { { person.id } } </td>
            <td> { { person.name } } </td>
        </tr>
        </tbody>
    </table>
    

</div>

<script src="/webjars/jquery/3.0.0/jquery.min.js"></script>
<script src="/webjars/bootstrap/4.4.1/js/bootstrap.min.js"></script>
<script src="/webjars/axios/0.19.0/dist/axios.min.js"></script>
<script src="/webjars/vue/2.6.11/vue.min.js"></script>
</body>
</html>
```

然后启动项目，访问http://localhost:8080/，是否出现![image-20200501174126609](https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200501174126609.png)

然后在自己的数据库表中设置一定数据，方便后面进行测试



## 建立PersonRestController，设置api

<img src="https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200501174222781.png" style="zoom:50%;" />

```java
package com.example_yangweidong.demo.controller;

import com.example_yangweidong.demo.model.Person;
import com.example_yangweidong.demo.service.PersonService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
@RequestMapping("/api/v1")
public class PersonRestController {
    @Autowired
    public PersonService personService;

    @GetMapping("/persons")
    public List<Person> findAll() {
        return personService.findAll();
    }
}

```



## 通过vue设置前端页面

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org" xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <link href='/webjars/bootstrap/4.4.1/css/bootstrap.min.css' rel='stylesheet'>

    <meta charset="UTF-8">
    <title>Home</title>
</head>
<body>

<div class="container" id="main">
    <table class="table table-striped table-bordered">
        <thead>
        <tr>
            <th>ID</th>
            <th>Name</th>

        </tr>
        </thead>
        <tbody>
        <tr v-for="person in persons">
            <td> { { person.id } } </td>
            <td> { { person.name } } </td>
        </tr>
        </tbody>
    </table>


</div>

<script src="/webjars/jquery/3.0.0/jquery.min.js"></script>
<script src="/webjars/bootstrap/4.4.1/js/bootstrap.min.js"></script>
<script src="/webjars/axios/0.19.0/dist/axios.min.js"></script>
<script src="/webjars/vue/2.6.11/vue.min.js"></script>
<script>
    var app = new Vue({
        el: "#main",
        data() {
            return {
                persons: null,
                person_id: "",
                person_name: "",
            }
        },
        mounted() {
            this.get();
        },
        methods: {
            //获取数据
            get: function() {
                axios
                    .get("http://localhost:8080/api/v1/persons")
                    .then(response => (this.persons = response.data))
            },
        }
    })
</script>
</body>
</html>
```





## 查看结果

![image-20200501183508315](https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200501183508315.png)





>参考链接：https://dev.to/brunodrugowick/complete-crud-with-spring-boot-vue-js-axios-fg1

# 部署github上的springboot项目

