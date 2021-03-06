---
layout:     post
title:      "define与inline的区别"
subtitle:   "difference between define and inline"
author:     "Jayn"
header-img: "/img/post-bg-03.jpg"
tags:
  - C++
---


## define

define就是简单的做一下文本的替换，没有类型的检查。并且这种简单的文本替换会经常出现不期望的问题。

```c++
#define ExpressionName(Var1,Var2) (Var1+Var2)*(Var1-Var2)
```
这种表达式形式宏形式与作用跟函数类似，但它使用预编译器，用复制宏代码的方式取代函数调用，省去了参数压栈、生成汇编语言的CALL调用、返回参数、执行return等过程，从而提高速度，使用上比函数高效。

但它只是预编译器上符号表的简单替换，不能进行参数有效性检测及使用C++类的成员访问控制。无法调试，无法操作类的私有成员变量.



## inline

`inline` 推出的目的，也正是为了取代上面的宏定义表达式形式，它消除了它的缺点，同时又很好地继承了它的优点。inline代码放入预编译器符号表中，高效；它是个真正的函数，调用时有严格的参数检测；它也可作为类的成员函数。c++的内联函数机制既具备宏代码的效率，又增加了安全性，且可自由操作类的数据成员。

如我们定义如下内敛函数：

```c++
inline const string & shorterString(const string &s1,const string &s2){
    return s1.size()<s2.size()?s1:s2;
}
```


当我们调用内联函数的时候：

```c++
cout<<shorterString(s1,s2).c_str()<<endl;
```

在编译时会展开为：

```c++
cout<<(s1.size()<s2.size()?s1:s2).c_str()<<endl;
```

从而消除了把`shorterString`写成普通函数的额外调用开支。

内联函数对编译器提出建议，是否进行宏替换，编译器有权拒绝。

内联函数是存在代码膨胀的问题，一般如果函数的代码较少，调用有又较频繁，则适合使用`inline`函数。

### 内敛函数使用时注意的问题
1. 一个文件中定义的内联函数在其他文件中不能使用，他们通常放在头文件中进行共享。
2. 内联函数应该简洁，只有几个语句，如果语句较多，不适合于定义为内联函数。
3. 内联函数体中，不能有循环语句、if语句或switch语句，否则，函数定义时即使有inline关键字，编译器也会把该函数作为非内联函数处理。
4. 内联函数要在函数被调用之前声明。否则编译器不会把它当作内联函数使用，而仅仅当作普通函数调用。