---
title: c++ tempalte
date: 2020-04-20 15:09:16
tags:
	- c++
---

泛型编程时软件工程中很重要一个思想，为了提高方法和代码的可重用性，泛型编程给了我们很大的帮助。在c++中泛型编程的实现机制就是通过c++的template来运行的。

<!-- more -->

# 使用template的函数

>在声明和定义模板函数的时候，必须将函数的声明和定义放在同一个文件中(一般都是放在头文件中)，因为这样c++在实例化必须要要知道模板的确切定义
>
>参考链接：https://blog.csdn.net/zhengqijun_/article/details/81159433

```c++
#include <iostream>
#include "generics.hpp"
int main(){
    std::cout << maxNumber<int>(3, 7) << std::endl;
    return 0;
}
```



```cpp
#ifndef generics_hpp
#define generics_hpp

#include <iostream>
template<class T>
T maxNumber(T n1, T n2) {
    if (n1 > n2) {
        return n1;
    }else
    {
        return n2;
    }
};

#endif /* generics_hpp */
```



# 使用模板类

```cpp
//generics.h
template<class T>
class Array {
private:
    T* pointer;
    int size;
public:
    Array(T array[], int s) {
        pointer = new T[s];
        size = s;
        for (int i = 0; i < s; i++) {
            pointer[i] = array[i];
        }
    }
    
    void print() {
        for (int i = 0; i < size; i++ ) {
            std::cout << pointer[i] << std::endl;
        }
    }
};


或者是可以这样
template<class T>
class Array {
private:
    T* pointer;
    int size;
public:
    Array(T array[], int s);
    void print();
};

template<class T>
Array<T>::Array(T array[], int s) {
        pointer = new T[s];
        size = s;
        for (int i = 0; i < s; i++) {
            pointer[i] = array[i];
       }
}

void Array<T>::print() {
  for (int i = 0; i < size; i++ ) {
            std::cout << pointer[i] << std::endl;
        }
}
//在其他地方使用的时候都要标明他是模板类
```

```cpp
//main.h
#include <iostream>
#include "generics.hpp"
int main(){
    int array[5] = {1, 2, 3, 4, 5};
    Array<int> a1(array, 5);
    a1.print();
}
```



# 多种类型的模板类

```cpp
#include <iostream> 
using namespace std; 
  
template <class T, class U> 
class A { 
    T x; 
    U y; 
  
public: 
    A() 
    { 
        cout << "Constructor Called" << endl; 
    } 
}; 
  
int main() 
{ 
    A<char, char> a; 
    A<int, double> b; 
    return 0; 
} 
```

