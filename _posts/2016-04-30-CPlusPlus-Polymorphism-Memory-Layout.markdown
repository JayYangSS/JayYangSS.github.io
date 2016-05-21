---
layout:     post
title:      "谈谈C++中的类存储模型"
subtitle:   "Polymorphism Memory Layout"
author:     "Jayn"
header-img: "/img/post-bg-05.jpg"
tags:
  - C++
---

前几天去摩根面试，然后被面试官问到有关C++中类的存储模型。虽然之前有看，但是还是理解不够深入，今天把这个问题好好的整理了一下，算是搞清楚了。

首先还是先结合代码来看一个例子吧：

```c++
class A{
public:
    virtual void f(){ cout << "A::f()" << endl; };
    virtual void g(){ cout << "A::g()" << endl; };
    virtual void h(){ cout << "A::h()" << endl; };
    int c;

private:
    int a;
    int b;
};

class B
{
public:
    virtual void f1(){ cout << "B::f1()" << endl; }
    virtual void g1(){ cout << "B::g1()" << endl; };
    virtual void h1(){ cout << "B::h1()" << endl; };
    int c1;

private:
    int a1;
    int b1;
};

class C :private A{
public:
    virtual void f(){ cout << "C::f()" << endl; };
    virtual void g(){ cout << "C::g()" << endl; };
    virtual void h2(){ cout << "C::h2()" << endl; };
};

class D :public A, private B{
public:
    void f(){ cout << "D::f()" << endl; };
    void g1(){ cout << "D::g1()" << endl; };
    virtual void h3(){ cout << "D::h()" << endl; };
    void p(){};

    virtual int getA(){ return 1; }
    virtual int getB(){ return 2; }
};
```

使用VS2013编译上述代码，注意为了能够了解类的内存布局结构，需要在`property->C/C++->CommandLine`中的其他选项处添加如下配置：

>/d1 reportAllClassLayout

这样可以看到所有类的内存布局，如果写上：

>/d1 reportSingleClassLayoutXXX（XXX为类名）

则只会打出指定类`XXX`的内存布局。

编译后我们看看输出的这些类的内存布局吧：

```c++
1>  class A size(16):
1>      +---
1>   0  | {vfptr}
1>   4  | c
1>   8  | a
1>  12  | b
1>      +---
1>  
1>  A::$vftable@:
1>      | &A_meta
1>      |  0
1>   0  | &A::f 
1>   1  | &A::g 
1>   2  | &A::h 
-----------------------------------------------------------
1>  class B size(16):
1>      +---
1>   0  | {vfptr}
1>   4  | c1
1>   8  | a1
1>  12  | b1
1>      +---
1>  
1>  B::$vftable@:
1>      | &B_meta
1>      |  0
1>   0  | &B::f1 
1>   1  | &B::g1 
1>   2  | &B::h1
-------------------------------------------------------------
1>  class C size(16):
1>      +---
1>      | +--- (base class A)
1>   0  | | {vfptr}
1>   4  | | c
1>   8  | | a
1>  12  | | b
1>      | +---
1>      +---
1>  
1>  C::$vftable@:
1>      | &C_meta
1>      |  0
1>   0  | &C::f 
1>   1  | &C::g 
1>   2  | &A::h 
1>   3  | &C::h2 
-----------------------------------------------------------------
1>  class D size(32):
1>      +---
1>      | +--- (base class A)
1>   0  | | {vfptr}
1>   4  | | c
1>   8  | | a
1>  12  | | b
1>      | +---
1>      | +--- (base class B)
1>  16  | | {vfptr}
1>  20  | | c1
1>  24  | | a1
1>  28  | | b1
1>      | +---
1>      +---
1>  
1>  D::$vftable@A@:
1>      | &D_meta
1>      |  0
1>   0  | &D::f 
1>   1  | &A::g 
1>   2  | &A::h 
1>   3  | &D::h3 
1>   4  | &D::getA 
1>   5  | &D::getB 
1>  
1>  D::$vftable@B@:
1>      | -16
1>   0  | &B::f1 
1>   1  | &D::g1 
1>   2  | &B::h1 
```

这几个类的存储模型如下图所示：

![memory model](http://7xniym.com1.z0.glb.clouddn.com/class%20memory%20model.jpg)

## 总结

通过上面的输出我们可以总结一下内存分布模型：

1. 类中存在虚函数的话，会在类的内存最开始位置存放一个虚表指针，然后才会存放成员变量；

2. 不管子类是否是以private方式继承还是以public方式继承父类，父类的成员变量都会存在于派生类的内存模型中；

3. 若子类继承多个父类，则在该子类内存模型中会存储来自每一个父类的虚表指针，存储顺序按照上图所示，即按照声明的父类继承顺序存储父类相应的内容

4. 若子类方法覆盖了父类方法中的虚函数，则在该父类的相应的虚函数表中将父类的虚函数方法替换为子类的方法

5. 若子类中出现新的虚函数（即父类中不存在的虚函数），则统一将这些新的方法依次放置在第一个虚函数表的后面

6. 非虚函数不会出现在虚函数表中，如上例中的类D的方法`p()`没有出现在虚函数表中。