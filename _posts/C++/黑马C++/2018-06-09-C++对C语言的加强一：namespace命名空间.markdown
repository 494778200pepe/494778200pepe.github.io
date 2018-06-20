---
layout: post
title:  "C++对C语言的加强一：namespace命名空间"
date:   2018-06-09 10:26:00 +0800
categories: C++
tags: 黑马C++
author: pepe
description: 『 C++对C语言的加强一：namespace命名空间 』
---
### include和using namespace

* #include用于引用其他文件的内容（如#include “a.h”），编译器在编译时，在使用include的文件中（如名为main.cpp），将include这句话替换为其文件内容，相当于物理上的复制替换。
* 当不同的两个文件中，使用了相同的函数名（a.h 和b.h两个文件中都有fun()），函数内容却不同，根据上文来看，如果使用include同时引用两个文件，则在编译时，mai.cpp文件中出现了两个fun()，因此重名，后面调用函数时也不知道调用的哪个函数。 
* 那么，如何避免上面出现的问题呢？这就需要使用命名空间namespace，把两个函数定义在不同的命名空间中，然后在需要调用函数的文件中加入using namespace（注意#include也需要写），将当前使用的函数链接到它声明的地方。

[C++：#include和using namespace - CSDN博客](https://blog.csdn.net/u013719339/article/details/80221899)

### C++命名空间基本常识

所谓namespace，是指`标识符的各种可见范围`。C++标准程序库中的所有标识符都被定义与一个名为std的namespace中。

* 一、`<iostream.h>` 是C语言的标准库，其实现是将标准库功能定义在全局空间里。
* 二、`<iostream.h>`和`<iostream>`是两个文件，C++为了和C区分，也为了正确使用命名空间，规定头文件不实用后缀.h。
* 三、当使用`<iostream.h>`时，相当于在C中调用库函数，使用的是全局命名空间，也就是早期的C++实现。
* 四、当使用`<iostream>`时，该头文件没有定义全局命名空间，必须使用`namespace std;`这样才能正确使用`cout`。

C中的命名空间

    * 在C语言中只有一个全局作用域
    * C语言中所有的全局标识符共享同一个作用域
    * 标识符之间可能发生冲突

C++中的命名空间

    * 命名空间将全局作用域分成不同的部分
    * 不同命名空间中的标识符可以同名而不会发生冲突
    * 命名空间可以互相嵌套
    * 全局作用域也叫默认命名空间
    
    
### 命名空间的使用

```
//方式一
std::cout << "hello world" << std::endl;

//方式二
using std::cout;
usint std::endl;

//方式三
using namespace std;
```
* 1、namespace可以嵌套
* 2、spaceA包含spaceB和spaceC，要使用spaceB中的成员，必须using 到 spaceB，不能只到spaceA，因为spaceC中可能也有同名成员。
* 3、可以在spaceA中调用`using namespace spaceB`，那么只需`using namespace spaceA`，就可以用到spaceB的成员。












### const

* 1、`const int a;`和`int const a;`一样，表示一个整型常量。
* 2、`const int* p`表示：const修饰的是 int*，表示指针p的指向不能变，不用使用*p来改变int的值，但p可以修改。
* 3、`int* const p`表示：const修饰的是 p，表示这个一个指针常量，但p的指向可以修改。
* 4、`const int* const p`表示一个指向常量整型的常量指针。














