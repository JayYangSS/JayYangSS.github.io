---
layout:     post
title:      "C++ 多态问题的梳理"
subtitle:   "Polymorphism of C++"
author:     "Jayn"
header-img: "/img/post-bg-05.jpg"
tags:
  - C++
---

C++的多态性问题是一个比较重要的概念，看了好多遍，但是经常忘记或模糊其中的一些细节，今天再看一遍，把这个总结一下，希望这样可以帮助自己巩固一下。

### C++多态的实现原理
先看一个例子：

{% highlight c++ %}

class Animal{
    public:
        void eat(){
            cout<<"animal eat"<<endl;
        }
}

class Fish:public Animal{
    public:
        void eat(){
            cout<<"fish eat"<<endl;
        }    
}

int main(){
    Fish fh;
    Animal *pAn=&fh;
    pAn->eat();
}
{% endhighlight %}

上面这段代码的输出为："animal eat",而不是"fish eat".
这是因为在编译器进行编译时，进行了`early-binding`.就是说，在创建`*pAn`时已经却确定了它的类型为`Animal`,将类型为`Fish`的实例指针强制转换为`Animal`类型，最后调用的自然也就是`Animal`的eat方法了。

要想实现多态，就需要使用虚函数来实现多态：

{% highlight c++ %}

class Animal{
    public:
        virtual void eat(){
            cout<<"animal eat"<<endl;
        }
}

class Fish:public Animal{
    public:
        void eat(){
            cout<<"fish eat"<<endl;
        }    
}

int main(){
    Fish fh;
    Animal *pAn=&fh;
    pAn->eat();
}
{% endhighlight %}

这时，输出结果就为`fish eat`.原因是使用`virtual`关键字后，就实现了迟绑定(late-binding)，即一种动态绑定技术，而不是在编译期间就确定了实例的类型。它的实现方法是使用虚表(virtual table)和虚表指针的方式来实现。创建每一个包含虚函数的类对应的实例都包含有一个虚表vtable,这个vtable是一个一维数组，存放的是这个类中的虚函数方法（注意虚表中存放的仅仅是虚函数方法，其他的非虚函数方法不再这个虚表中）。

如何定位每个虚表呢？编译器为每一个含有虚函数的类的对象实例提供一个虚表指针vptr.在程序运行时，根据对象实例的类型来初始化vptr,从而让vptr正确的指向所属类的虚表。在上例中，pAn指向的对象类型为`Fish`,所以对应的vptr指向的是`Fish`类的`vtable`,因此调用`pAn->eat()`调用的就是`Fish`类中vtable所指的Fish类的方法eat。

需要注意的是，上述的vtable和vptr都是在对象初始化后才产生的，而这个初始化的过程是在构造函数中完成的，因此构造函数不可以是虚函数！

---

## 一个有趣的问题

看下面的代码：

{% highlight c %}
#include<iostream>
using namespace std;
class B0//基类BO声明
{
public://外部接口
virtual void display()//虚成员函数
{
    cout<<"B0::display0"<<endl;}
};
class B1:public B0//公有派生
{
public:
    void display() { cout<<"B1::display0"<<endl; }
};
class D1: public B1//公有派生
{
public:
    void display(){ cout<<"D1::display0"<<endl; }
};
void fun(B0 ptr)//普通函数
{
    ptr.display();
}
int main()//主函数
{
    B0 b0;//声明基类对象和指针

    B1 b1;//声明派生类对象
    
    D1 d1;//声明派生类对象
    
    fun(b0);//调用基类B0函数成员
    
    fun(b1);//调用派生类B1函数成员
    
    fun(d1);//调用派生类D1函数成员

}
{% endhighlight %}

上面这段代码的输出为什么呢？答案是：

>B0::display() B0::display() B0::display()

为啥不是：B0::display() B1::display() D1::display()？为啥没有实现多态？原因是就是由于`fun`函数的参数是不是按地址传值，这样会转化为基类对象，直接调用基类的成员函数，如果是指针传递，改为B0 *ptr，ptr->display()，可以实现多态这样的话就无法进行动态绑定。

虚函数的动态绑定仅在基类指针或引用绑定派生类对象时发生，fun的形参不是指针，所以调用哪个版本的函数编译时就已经确定，根据形参静态类型确定调用B0的成员。

### 总结

C++的多态性用一句话概括就是：在基类的函数前加上`virtual`关键字，在派生类中重写该函数，运行时将会根据对象的实际类型来调用相应的函数。如果对象类型是派生类，就调用派生类的函数；如果对象类型是基类，就调用基类的函数。

如果在父类中的某个方法上添加了关键字`virtual`，那么它的所有子类的该方法不用再写关键字`virtual`，他们都是虚函数，都可以对父类进行方法覆盖。