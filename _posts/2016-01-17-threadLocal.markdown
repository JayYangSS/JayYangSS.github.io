---
layout:     post
title:      "ThreadLocal的用法"
subtitle:   "learning ThreadLocal"
author:     "Jayn"
header-img: "/img/about-bg.jpg"
tags:
  - Java
---

ThreadLocal是表示线程的局部变量，ThreadLocal为每个使用该变量的线程提供独立的变量副本，所以每一个线程都可以独立地改变自己的副本，而不会影响其它线程所对应的副本。它支持泛型，几个方法的使用方法如下：

1. `public T get()`，获取当前线程的线程局部变量的副本。
2. `public void set(T value)`，设置当前线程的线程局部变量的副本的值
3. `public void remove()`,删除当前线程的局部变量的值删除，减少内存，需要指出的是，当线程结束后，对应该线程的局部变量将自动被垃圾回收，所以显式调用该方法清除线程的局部变量并不是必须的操作，但它可以加快内存回收的速度。
4. `protected T initialValue()`,该方法是为了让子类覆盖继承而设计的。这个方法是一个延迟调用方法，在线程第一次调用get()或set()方法时才执行，且只执行一次。缺省值返回null.


