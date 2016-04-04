---
layout:     post
title:      "感到智商不足了"
subtitle:   "Coding Pratice"
author:     "Jayn"
header-img: "/img/post-bg-05.jpg"
tags:
  - Algorithm
---

### 题目
一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。 


### 我的解法
问题看起来很easy嘛，直接开撸
用$f(n)$表示n个台阶时的跳法，那么很容易写出表达式：

$$
f(n)=\begin{cases}
0, & \text{if n=0}\\
1, & \text{if n=1}\\
f(n)=f(n-1)+f(n-2)+....+f(0)+1, & \text{if n>1}
\end{cases}
$$


```c++
class Solution {
public:
    int jumpFloorII(int number) {
        
        int result=0;
        if(number==0)return 0;
        if(number==1)return 1;
         
        vector<int> container;
        container.push_back(0);
        container.push_back(1);
         
        //get the f(0) to f(n-1)
        for(int i=2;i<number;i++){
            int tmp=0;
            for(int j=0;j<i;j++){
                tmp+=container[j];
            }
            container.push_back(tmp+1);
        }
        
        for(int i=0;i<number;i++){
            result+=container[i];
        }
        return result+1;
    }
};
```

但是我看到了一个更吊的代码，只用一行就搞定了：

```c++
class Solution {
public:
    int jumpFloorII(int number) {
       return  1<<--number; 
    }
};
```

刚刚看到这行代码的时候，一口老血要吐出来了，这不是藐视我的智商啊。。
其实，写出了上面的公式，再仔细整理一下就可以得出如下关系式：

$$f(n)=2f(n-1)$$

看来还是高中数列没学好啊。以后要多注意观察啊，不然吭哧吭哧写这么多代码，人家一行就搞定了，真是个忧伤的故事。