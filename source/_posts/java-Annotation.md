---
title: java-Annotation
date: 2020-04-22 22:16:28
tags:
	- java
---

### java annotation

> 参考文章：https://dzone.com/articles/how-annotations-work-java

```java
@Override
public String toString() {
	return "This is String Representation of current object.";
}
```

我已经覆盖了 上面的代码中的 `toString()` 方法并使用了 `@Override`注释。即使我不放 `@Override`，代码也可以正常工作而没有任何问题。那么，此注释的优点是什么？ `@Override` 告诉编译器此方法是重写的方法（有关该方法的元数据），并且如果父类中不存在任何这样的方法，则抛出编译器错误（方法不会从其超类中重写方法）。现在，如果我犯了一个印刷错误，并且将方法名称用作 `toStrring() {double r}`，并且如果我不使用 `@Override`，则我的代码将成功编译并执行，但是结果将与我接受的结果不同。所以现在，我们了解了什么是批注，但是阅读正式定义还是不错的

注释是一种特殊的Java构造，用于装饰类，方法，字段，参数，变量，构造函数或包。它是JSR-175选择提供元数据的工具。

<!-- more -->

所以java注解的作用是注释，只不过这个注释不是给程序员看的，而是给编译器看的。告诉编译器这个方法的作用，以便编译器可以帮助检查或者是执行代码。



### Annotation是如何运行的

@Override似乎有问题；它什么也没做--它只是检查父类中是否定义了一个方法。那么，不要惊讶；我不是在跟你开玩笑。Override注释的定义只有那么多代码。这是最需要理解的部分，我在此重申一下。注释只是元数据，不包含任何业务逻辑。虽然很难消化，但这是事实。如果注释不包含逻辑，那么一定是别人在做什么，而这个人就是这个注释元数据的消费者。注释只提供了关于属性（类/方法/包/字段）的信息。消费者是一个读取这些信息，然后执行必要的逻辑的代码。



|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![image-20200422225106784](https://raw.githubusercontent.com/a11enyang/Picture/master/img2/image-20200422225106784.png) | ![image-20200422225326650](https://raw.githubusercontent.com/a11enyang/Picture/master/img2/image-20200422225326650.png) |

