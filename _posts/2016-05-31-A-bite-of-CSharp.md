---
layout:     post
title:      "走马观花看C#"
subtitle:   "A bite of C#"
author:     "Jayn"
header-img: "/img/post-bg-05.jpg"
tags:
  - C#
---

要去微软实习了，微软那边的C#感觉要占主导地位啊，所以要开始学习一下C#。下面的内容是我参考[C#菜鸟教程](http://www.runoob.com/csharp/csharp-tutorial.html)快速的撸了一遍语言特性，记录了其中的一些重点吧，关于之后的一些更加细节高阶的东西，在今后的实践中再慢慢体会吧。


## C#初探
- C#程序源文件扩展名为`*.cs`
- C#中的一个namespace中包含多个类
- Main 方法，是所有 C# 程序的 入口点。Main 方法说明当执行时 类将做什么动作
- 与 Java 不同的是，文件名可以不同于类的名称
- 键入 csc helloworld.cs 并按下 enter 键来编译代码
- using 关键字用于在程序中包含命名空间。一个程序可以包含多个 using 语句。一般程序第一句都是：

```
using System;
```

C#中的变量命名不能与C#中的关键字重名，如果一定要用相同的名字，要在前面加上@符号

### C#命名空间
命名空间的设计目的是为了提供一种让一组名称与其他名称分隔开的方式。在一个命名空间中声明的类的名称与另一个命名空间中声明的相同的类的名称不冲突。

#### namespace的嵌套
```
namespace first_space
{
   class abc
   {
      public void func()
      {
         Console.WriteLine("Inside first_space");
      }
   }
   namespace second_space
   {
      class efg
      {
         public void func()
         {
            Console.WriteLine("Inside second_space");
         }
      }
   }   
}
```
使用时，`first_space.abc(),first_space.seconf_space.efg()`


### C#数据类型
C#的数据类型包括以下三种：
1. 值类型：基本的数据类型都是从类 System.ValueType 中派生的。表达式 sizeof(type)产生以字节为单位存储对象或类型的存储尺寸.
2. 引用类型:引用类型不包含存储在变量中的实际数据，但它们包含对变量的引用。
换句话说，它们指的是一个内存位置。使用多个变量时，引用类型可以指向一个内存位置。如果内存位置的数据是由一个变量改变的，其他变量会自动反映这种值的变化。内置的 引用类型有：object、dynamic 和 string。
3. 指针类型:和C/C++中的指针类型一样

### C#类型转换
1. 隐式类型转换 - 这些转换是 C# 默认的以安全方式进行的转换。例如，从小的整数类型转换为大的整数类型，从派生类转换为基类。
2. 显式类型转换 - 这些转换是通过用户使用预定义的函数显式完成的。显式转换需要强制转换运算符。

### C#的输入输出
使用

```
Console.WriteLine()//输出到控制台
Console.ReadLine()//从控制台读入用户输入，只接受字符串格式数据
```

### C#封装

#### 访问修饰符
如果没有指定任何访问修饰符，则使用类成员的默认修饰符，即为`private`

- Public
- Private:只有同一个类中的函数可以访问他们的私有成员变量和私有成员函数，即使类的实例也不能访问它的私有成员
- Protected：允许子类访问它的基类的成员变量和成员函数
- Internal：允许一个类将其成员变量和成员函数暴露给当前程序中的其他函数和对象。换句话说，带有 internal 访问修饰符的任何成员可以被定义在该成员所定义的应用程序内的任何类或方法访问。
- Protected internal：允许一个类将其成员变量和成员函数对同一应用程序内的子类以外的其他的类对象和函数进行隐藏。这也被用于实现继承

### C#可空类型(Nullable)
`nullable类型`（可空类型），可空类型可以表示其基础值类型正常范围内的值，再加上一个 null 值。例如，Nullable< Int32 >，读作"可空的 Int32"，可以被赋值为 -2,147,483,648 到 2,147,483,647 之间的任意值，也可以被赋值为 null 值。类似的，Nullable< bool > 变量可以被赋值为 true 或 false 或 null。

```
bool b1=null;//错误，不可以给bool类型的变量赋值为null
bool? b2=null;//正确，此时b2的类型为nullable类型
```

声明一个**nullable**类型的语法如下：

```
< data_type> ? <variable_name> = null;
```

### Null 合并运算符（ ?? ）
Null 合并运算符用于定义可空类型和引用类型的默认值。当合并运算符前面的值为空，则使用运算符后面的值作为预设值赋值给指定变量。

```
 double? num1 = null;
         double? num2 = 3.14157;
         double num3;
         num3 = num1 ?? 5.34;      
         Console.WriteLine("num3 的值： {0}", num3);
         num3 = num2 ?? 5.34;
         Console.WriteLine("num3 的值： {0}", num3);
         Console.ReadLine();
```

### C#数组
当创建一个数组时，C# 编译器会根据数组类型隐式初始化每个数组元素为一个默认值。例如，int 数组的所有元素都会被初始化为 0。

Array类在 System 命名空间中定义，是所有数组的基类

C#中的数组主要保留三种类型：
1. 一维数组
2. 多维数组（最简单的就是二维数组）
3. 交错数组（即数组的数组）

数组的声明示例如下：

```
public static void Main()
        {
            int[] a = { 1, 2, 3, 4, 5 };
            int[,] b = { { 1, 2 }, { 3, 4 } };
            int[][] c = { new int[] { 1, 2, 3, 4 }, new int[] { 5, 6 }, new[] { 7 }, new int[3] { 1, 4, 2 } };
            int[] n = new int[10];

            //initialization
            for (int i = 0; i < n.Length; i++)
                n[i] = i;

            Console.WriteLine("访问a中的元素：");
            foreach (int x in a)
            {
                Console.WriteLine(x);
            }

            Console.WriteLine("访问c中的元素：");
            foreach (int[] x in c) 
            {
                Console.WriteLine(x);
                for (int i = 0; i < x.Length; i++)
                    Console.WriteLine(x[i]);
            }
            Console.ReadKey();
        } 
```

### C#中的结构体
与C/C++中的结构体不同的地方：

- 结构可带有方法、字段、索引、属性、运算符方法和事件。
- 结构可定义构造函数，但不能定义析构函数。但是，您不能为结构定义默认的构造函数。默认的构造函数是自动定义的，且不能被改变。
- 与类不同，结构不能继承其他的结构或类。
- 结构不能作为其他结构或类的基础结构。
- 结构可实现一个或多个接口。
- 结构成员不能指定为 abstract、virtual 或 protected。
- 当您使用 New 操作符创建一个结构对象时，会调用适当的构造函数来创建结构。与类不同，结构可以不使用 New 操作符即可被实例化。
- 如果不使用 New 操作符，只有在所有的字段都被初始化之后，字段才被赋值，对象才被使用。

### C#中枚举类型
定义语法为：

```
enum <enum_name>
{ 
    enumeration list 
};

enum Days { Monday, Tuesday, Thursday, Friday, Saturday };
Console.WriteLine("week start:{0}", Days.Monday);
```

输出为：`week start:Monday`

### C#中的类
1. C#中类的默认访问限制符为`internal`，而类中的成员默认的访问限制符为`private`
2. 声明方式跟C/C++ ,java很相似

```
<access specifier> class  class_name 
{
    // member variables
    <access specifier> <data type> variable1;
    <access specifier> <data type> variable2;
    ...
    <access specifier> <data type> variableN;
    // member methods
    <access specifier> <return type> method1(parameter_list) 
    {
        // method body 
    }
    <access specifier> <return type> method2(parameter_list) 
    {
        // method body 
    }
    ...
    <access specifier> <return type> methodN(parameter_list) 
    {
        // method body 
    }
}
```

### C#的析构函数
析构函数不可以被继承和重载

### C#的静态变量
关键字 static 意味着类中只有一个该成员的实例。静态变量用于定义常量，因为它们的值可以通过直接调用类而不需要创建类的实例来获取。静态变量可在成员函数或类的定义外部进行初始化。您也可以在类的定义内部初始化静态变量。

如果在声明static变量时没有赋值，则会用默认值初始化静态变量。

### C#的类继承

```
<acess-specifier> class <base_class>
{
 ...
}
class <derived_class> : <base_class>
{
 ...
}
```

子类可以调用父类的构造方法，方式如下：

```
//Tabletop是Rectangle的子类
public Tabletop(double l, double w) : base(l, w)
    { }
```

C#不支持多重继承，但是可以继承多个接口来实现多重继承
**注意：**当一个类继承与一个父类和多个接口时，要将父类写在最前，之后再是各个接口

### C#接口
接口使用 interface 关键字声明，它与类的声明类似。接口声明默认是 public 的。下面是一个接口声明的实例:

```
public interface ITransactions
{
   // 接口成员
   void showTransaction();
   double getAmount();
}
```

### C#异常
C# 异常处理时建立在四个关键词之上的：try、catch、finally 和 throw。
示例如下：

```
try
{
   // 引起异常的语句
}
catch( ExceptionName e1 )
{
   // 错误处理代码
}
catch( ExceptionName e2 )
{
   // 错误处理代码
}
catch( ExceptionName eN )
{
   // 错误处理代码
}
finally
{
   // 要执行的语句
}
```

#### C# 中的异常类

C# 异常是使用类来表示的。C# 中的异常类主要是直接或间接地派生于 `System.Exception` 类。`System.ApplicationException` 和 `System.SystemException `类是派生于` System.Exception `类的异常类。

- `System.ApplicationException` 类支持由应用程序生成的异常。所以程序员定义的异常都应派生自该类。

- `System.SystemException` 类是所有预定义的系统异常的基类。


---
下面的就是C#的高级特性了。

### C#的抽象属性
抽象类可拥有抽象属性，这些属性应在派生类中被实现。下面的程序说明了这点：

```
namespace CSharpLearning
{
    abstract class  AbstractProperty
    {
        public abstract string Name
        {
            get;
            set;
        }
    }

    class DeriveCl:AbstractProperty
    {
        private string name;
        public override string Name
        {
            get
            {
                return name;
            }
            set
            {
                name = value;
            }
        }
    }

    class AbstractPropertyDemo
    {
        
        public static void Main(){
            AbstractProperty stu = new DeriveCl();
            stu.Name = "Jayn";

            Console.WriteLine(stu.Name);
            Console.ReadKey();
        }
        
    }
}

```

### C#索引器（Indexer）
 允许一个对象可以像数组一样被索引。当您为类定义一个索引器时，该类的行为就会像一个 虚拟数组（virtual array） 一样。您可以使用数组访问运算符（[ ]）来访问该类的实例。
 
#### 语法
一维索引器的语法如下：

```
element-type this[int index] 
{
   // get 访问器
   get 
   {
      // 返回 index 指定的值
   }

   // set 访问器
   set 
   {
      // 设置 index 指定的值 
   }
}
```

### C#的委托
C#中的委托类似于C/C++中的函数指针。委托（Delegate） 是存有对某个方法的引用的一种引用类型变量。引用可在运行时被改变。
委托（Delegate）特别用于实现事件和回调方法。所有的委托（Delegate）都派生自 System.Delegate 类

#### 声明委托

```
delegate <return type> <delegate-name> <parameter list>
public delegate int MyDelegate (string s);
```

#### 委托的实例化
一旦声明了委托，就需要使用声明的委托类型来进行实例化。委托对象必须使用 new 关键字来创建，且与一个特定的方法有关。当创建委托时，传递到 new 语句的参数就像方法调用一样书写，但是不带有参数。例如：

```
public delegate void printString(string s);
...
printString ps1 = new printString(WriteToScreen);
printString ps2 = new printString(WriteToFile);
```

#### 委托的多播
委托对象可使用 "+" 运算符进行合并。一个合并委托调用它所合并的两个委托。只有相同类型的委托可被合并。"-" 运算符可用于从合并的委托中移除组件委托。 

多播的用途举例：

```
using System;
using System.IO;

namespace DelegateAppl
{
   class PrintString
   {
      static FileStream fs;
      static StreamWriter sw;
      // 委托声明
      public delegate void printString(string s);

      // 该方法打印到控制台
      public static void WriteToScreen(string str)
      {
         Console.WriteLine("The String is: {0}", str);
      }
      // 该方法打印到文件
      public static void WriteToFile(string s)
      {
         fs = new FileStream("c:\\message.txt",
         FileMode.Append, FileAccess.Write);
         sw = new StreamWriter(fs);
         sw.WriteLine(s);
         sw.Flush();
         sw.Close();
         fs.Close();
      }
      // 该方法把委托作为参数，并使用它调用方法
      public static void sendString(printString ps)
      {
         ps("Hello World");
      }
      static void Main(string[] args)
      {
         printString ps1 = new printString(WriteToScreen);
         printString ps2 = new printString(WriteToFile);
         sendString(ps1);
         sendString(ps2);
         Console.ReadKey();
      }
   }
}
```

### C#的事件
事件感觉是对委托（Delegate）的封装，时间声明后，不能在类外直接访问事件，只能由本类的方法访问本类的事件(event)。在类外只能使用`+=`或`-=`注册或去除事件处理函数。

```
namespace CSharpLearning
{
    public delegate void GreetDelegate(string name);
    class GreetingDemo
    {
        public static void EnglishGreeting(string name)
        {
            Console.WriteLine("Hi!" + name);
        }
        public static void ChineseGreeting(string name)
        {
            Console.WriteLine("你好！" + name);
        }
    }

    class GreetManager
    {
        //声明一个事件类似于声明一个进行了封装的委托类型的变量而已，无论是否声明为public，都无法在类外直接注册
        public event GreetDelegate GreetEvent;
        public void GreetMethod(string name)
        {
            if (GreetEvent != null)
            {
                GreetEvent(name);
            }
        }
    }

    class ExecuteDemo
    {
       
        
        public static void Main()
        {
            GreetManager gm = new GreetManager();
            //gm.GreetEvent = GreetingDemo.EnglishGreeting;//在类外不能直接赋值
            gm.GreetEvent += GreetingDemo.EnglishGreeting;
            gm.GreetEvent += GreetingDemo.ChineseGreeting;
            //gm.GreetEvent("Jayn");//不能在类外直接访问event
            gm.GreetMethod("Jayn!");
            Console.ReadKey();
        }
    }
}

```

### C#泛型
泛型类中不能有Main方法作为程序的入口点。

#### 泛型方法

```
public void swap<T>(ref T a,ref T b);
```

>**ref与out的区别**

>首先：两者都是按地址传递的，使用后都将改变原来参数的数值。

>其次：ref可以把参数的数值传递进函数，但是out是要把参数清空，就是说你无法把一个数值从out传递进去的，out进去后，参数的数值为空，所以你必须初始化一次。这个就是两个的区别，或者说就像有的网友说的，ref是有进有出，out是只出不进。


#### 泛型委托

```
delegate T NumberChanger<T>(T n);
```

### C#匿名方法（Anonymous methods）
匿名方法提供了一种传递代码块作为委托参数的技术。匿名方法是没有名称只有主体的方法。在匿名方法中您不需要指定返回类型，它是从方法主体内的 return 语句推断的。

```
delegate void NumberChanger(int n);
...
NumberChanger nc = delegate(int x)
{
    Console.WriteLine("Anonymous Method: {0}", x);
};
```

下面的代码展示了通过委托调用匿名方法和命名方法：

```
delegate void NumberChanger(int n);
...
static void Main(string[] args)
        {
            // 使用匿名方法创建委托实例
            NumberChanger nc = delegate(int x)
            {
               Console.WriteLine("Anonymous Method: {0}", x);
            };
            
            // 使用匿名方法调用委托
            nc(10);

            // 使用命名方法实例化委托
            nc =  new NumberChanger(AddNum);
            
            // 使用命名方法调用委托
            nc(5);

            // 使用另一个命名方法实例化委托
            nc =  new NumberChanger(MultNum);
            
            // 使用命名方法调用委托
            nc(2);
            Console.ReadKey();
        }
```

### C# 不安全代码
C#中的不安全代码是使用了指针变量的代码块。当一个代码块使用 `unsafe` 修饰符标记时，C# 允许在函数中使用指针变量。

编译不安全代码需要使用如下命令：

```
csc /unsafe prog1.cs
```

#### 指针变量
与C/C++中的指针变量是一致的。

`unsafe`可以用在方法上，页可以用在部分代码块中。

```
 public static void Main()
      {
         unsafe
         {
            int var = 20;
            int* p = &var;
            Console.WriteLine("Data is: {0} " , var);
            Console.WriteLine("Data is: {0} " , p->ToString());
            Console.WriteLine("Address is: {0} " , (int)p);
         }
         Console.ReadKey();
      }
      
      static unsafe void Main(string[] args)
        {
            int var = 20;
            int* p = &var;
            Console.WriteLine("Data is: {0} ",  var);
            Console.WriteLine("Address is: {0}",  (int)p);
            Console.ReadKey();
        }
```

#### 使用指针访问数组元素

在 C# 中，数组名称和一个指向与数组数据具有相同数据类型的指针是不同的变量类型。例如，`int *p` 和 `int[] p` 是不同的类型。您可以增加指针变量p，因为它在内存中不是固定的，但是数组地址在内存中是固定的，所以您不能增加数组 p。

因此，如果您需要使用指针变量访问数组数据，可以像我们通常在 C 或 C++ 中所做的那样，使用 fixed 关键字来固定指针。
```
using System;
namespace UnsafeCodeApplication
{
   class TestPointer
   {
      public unsafe static void Main()
      {
         int[]  list = {10, 100, 200};
         fixed(int *ptr = list)

         /* 显示指针中数组地址 */
         for ( int i = 0; i < 3; i++)
         {
            Console.WriteLine("Address of list[{0}]={1}",i,(int)(ptr + i));
            Console.WriteLine("Value of list[{0}]={1}", i, *(ptr + i));
         }
         Console.ReadKey();
      }
   }
}
```

### C#线程

#### 创建线程
使用`ThreadStart`委托来调用线程的执行函数，使用委托创建线程。注意`ThreadStart`委托只能传入函数类型为`void functionName(void)`的函数，也就是是无参数传递的函数。如果想给创建的线程传递函数，则需要使用`ParameterizedThreadStart`委托类型。该委托类型可以调用的函数类型为`void functionName(object)`,函数传递的参数可以是1一个值，也可以是一个自定义的对象，在Thread.Start()中传递该对象。

#### 线程的销毁

Abort() 方法用于销毁线程。

通过抛出 `threadabortexception` 在运行时中止线程。这个异常不能被捕获，如果有 `finally` 块，控制会被送至 `finally` 块。


下面的程序是上面所述内容的一个示例：

```
namespace CSharpLearning
{
    class AddClass
    {
        private int a;
        private int b;
        public AddClass(int val1, int val2)
        {
            a = val1;
            b = val2;
        }
        public static void add(object o)
        {
            int val1 = ((AddClass)o).a;
            int val2 = ((AddClass)o).b;
            Console.WriteLine("sum is {0}", val1 + val2);
        }
    }
    


    class ThreadTest
    {
        public static void sayHi()
        {
            Console.WriteLine("Hi!");
            Thread.Sleep(2000);
            Console.WriteLine("Sleep complete!");
        }


        public static void CallToChildThread()
        {
            try
            {

                Console.WriteLine("Child thread starts");
                // 计数到 10
                for (int counter = 0; counter <= 10; counter++)
                {
                    Thread.Sleep(500);
                    Console.WriteLine(counter);
                }
                Console.WriteLine("Child Thread Completed");

            }
            catch (ThreadAbortException e)
            {
                Console.WriteLine("Thread Abort Exception");
            }
            finally
            {
                Console.WriteLine("Couldn't catch the Thread Exception");
            }

        }



        public static void Main()
        {
            ThreadStart threadStart = new ThreadStart(sayHi);
            Thread th1 = new Thread(threadStart);
            th1.Start();
            //传递参数
            AddClass addclass = new AddClass(10, 23);
            ParameterizedThreadStart paraThread = new ParameterizedThreadStart(AddClass.add);
            Thread th2 = new Thread(paraThread);
            th2.Start(addclass);


            Console.WriteLine("Main thread");
            
            //test thread abortion
            ThreadStart childref = new ThreadStart(CallToChildThread);
            Console.WriteLine("In Main: Creating the Child thread");
            Thread childThread = new Thread(childref);
            childThread.Start();
            // 停止主线程一段时间
            Thread.Sleep(2000);
            // 现在中止子线程
            Console.WriteLine("In Main: Aborting the Child thread");
            childThread.Abort();
            Console.ReadKey();
        } 
        
    }
}

```