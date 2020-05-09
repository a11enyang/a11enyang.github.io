---
title: java-generics
date: 2020-04-20 12:53:45
tags:
	- java
---

![image-20200430094306932](https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200430094306932.png)

<!-- more -->

# 什么时候使用java泛型

当只需要关注数据而不是数据类型的时候，只需要关注内容，那么就是可以使用泛型的时候。

在编译的时候会自动识别并且设置数据类型，减少传统方法检查数据类型时候所花费的时间。



# 泛型类

```cpp
public class GenericClass<T1, T2> {
    T1 o1;
    T2 o2;
    public GenericClass(T1 o1, T2 o2) {
        this.o1 = o1;
        this.o2 = o2;
    }
    public void show() {
        System.out.println(o1);
        System.out.println(o2);
    }
}
```



# 泛型接口

```cpp
interface GenericInterface<T1 ,T2> {
  T1 function1(T1 x);
  T2 function(T2 y);
}
public class A implements GenericInterface<String, Integer> {
  public T1 function() {
    //执行代码
  }
  public T2 function() {
    //执行代码
  }
}
```





# 泛型函数

![image-20200420161800421](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img2/image-20200420161800421.png)

需要在函数头进行声明

# 泛型构造器

![image-20200420161909403](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img2/image-20200420161909403.png)