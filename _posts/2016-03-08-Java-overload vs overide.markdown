---
layout:     post
title:      "Java中的overload与overide"
subtitle:   "Effective Java"
author:     "Jayn"
header-img: "/img/post-bg-05.jpg"
tags:
  - Java
---

### 1.重载（overload）
重载(overloading)是在一个类里面，方法名字相同，而参数不同。返回类型呢？可以相同也可以不同。
每个重载的方法（或者构造函数）都必须有一个独一无二的参数类型列表。
只能重载构造函数，而不能够覆盖构造函数。
方法的重载会导致有很多相同名称的方法，那么怎么决定是调用的哪个方法呢？就是根据方法的传入参数区别来决定使用哪个方法。

需要强调的一点是：java决定调用哪个重载方法是在编译期间根据参数类型做出决定的。对于重载方法的选择是静态的。
看一个例子：

```java
public class CollectionClassifier{  
    public static String classify(Set<?> s){  
    return “Set”;  
}  
public static String classify(List<?> l){  
    return “List”;  
}  
public static String classify(Collection<?> c){  
    return “Unknown Collection”;  
}  
public static void main(String[] args){  
    Collection<?>[] collections = {  
        new HashSet<String>(),  
        new ArrayList<BigInteger>(),  
        new HashMap<String>()  
};  
        for(Collection<?> c : collections){  
    System.out.println(classify(c));  
}  
}
}  
```

上面这段程序的本意是希望通过方法的重载来自动识别相应的集合类型，但是很不幸，最后的输出是打印三次`Unknown Collection`。因为在编译时，三个实例的编译类型都是一样的：` Collection<?>`.虽然它们三个运行时类型是不同的，但是这并不能影响到对重载方法的选择，因为这个过程在编译时就已经决定。

---

### 2.覆盖（overide）
覆盖是多态的实现，就是子类重写父类的方法，方法名，参数，返回类型都必须一致。对于被覆盖方法的选择是动态的。需要说明的是，这个方法调用的是哪个是在运行过程中动态决定的，而不是在编译期间根据实例的类型静态决定调用哪个方法的，这点是跟`overload`的方法选择调用方式是不同的。

看一个例子：

```java
class Wine {
    String name(){return "wine";}
}

class SparklingWine extends Wine{
    String name(){return "SparklingWine";}
}

class Champagne extends SparklingWine{
    String name(){return "Champagne";}
}

public class Overriding{
    public static main(String []args){
        Wine[] wines={new Wine(),new SparklingWine(),new Champagne()};
        for(Wine wine:wines){
            System.out.println(wine.name);
        }
    }
}
```

输出为"wine","SparklingWine","Champagne".虽然编译时类型均为Wine，但是最后调用的是自己对应的类型方法。因为覆盖的方法调用取决于运行时类型。