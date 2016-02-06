---
layout:     post
title:      "交通信号灯识别"
subtitle:   "using blob analysis method"
author:     "Jayn"
header-img: "/img/post-bg-06.jpg"
tags:
  - OpenCV
  - Computer Vison
---

今天尝试使用blob analysis的方式来进行交通信号灯的识别。信号灯的种类其实不多，就是圆形和箭头型信号灯，箭头型信号灯又分左转，右转和前行信号灯。之前使用的信号灯识别的方法是使用HOG+SVM的方法，但是想想，这样有点杀鸡使用宰牛刀的感觉了。HOG特诊是一种相对来说比较复杂的特征了，它包含了较多的物体形状纹理等特征。但是信号等的种类比较少，且区分度比较大，因此使用blob analysis的方法就足够了。

我的想法是这样的：

1. 首先区分出圆形信号灯和箭头型信号灯：分析圆形和箭头型信号灯的特征，可以发现，圆形信号灯发光单元的轮廓是凸的，而箭头型信号灯的发光单元轮廓是凹的，因此检发光单元轮廓的凹凸性就可以将圆形信号灯检测出来；
2. 区分三种箭头型信号灯：这个可以使用发光单元区域的重心的位置进行区分：左转信号灯的发光单元重心是在给定轮廓外接矩形框的偏左位置；右转信号灯的发光单元重心是在给定轮廓外接矩形框的偏右位置；当然，前行信号灯的发光单元重心是在给定轮廓外接矩形框的偏上位置

基于上述思想，我们给出信号灯的识别的算法：

{% highlight cpp %}
int RecognizeLight(Mat segImg)
{   
    Mat edge;
    vector<vector<Point>> contour;
    const int cols = segImg.cols;
    const int rows = segImg.rows;
    const double y_epsTh = 1;
    const double x_epsTh = 1;
    int center_x = cols / 2;
    int center_y = rows / 2;
    double x_eps = 0;
    double y_eps = 0;

    //define the recognition result
    const int circular_go = 0;
    const int direction_left = 1;
    const int direction_right = 2;

    //find contours
    imshow("segImg",segImg);
    waitKey(1);
    Canny(segImg, edge, 0, 50, 5);
    findContours(edge, contour, CV_RETR_EXTERNAL, CV_CHAIN_APPROX_SIMPLE);

    //use the convexity to determine whether it is the arrow TL
    bool isConvex = isContourConvex(contour[0]);
    if (isConvex){
        return 0;//return the circular recognition result
    }
    
    //calculate the moments od the TL
    Moments TLMoments = moments(segImg, true);
    int gravityCenter_x = (int)(TLMoments.m10 / TLMoments.m00);
    int gravityCenter_y = (int)(TLMoments.m01 / TLMoments.m00);
    y_eps = gravityCenter_y - center_y;
    x_eps = gravityCenter_x - center_x;

    //return the recogintion result
    if (y_eps > y_epsTh&&abs(x_eps)<x_epsTh){
        return circular_go;
    }
    if (x_eps<(0 - x_epsTh) && abs(y_eps) < y_epsTh){
        return direction_left;//return the left arrow recognition result
    }
    if (x_eps>x_epsTh&&abs(y_eps) < y_epsTh){
        return direction_right;//return the right arrow recognition result
    }
}
{% endhighlight %}


下面的这段代码中，`Mat segImg`是提取到的信号灯发光单元，为单通道图像，如下图中间的图像
上所示：
![TLREC1](http://7xniym.com1.z0.glb.clouddn.com/TLREC1.png)
![TLREC2](http://7xniym.com1.z0.glb.clouddn.com/TLREC2.png)

上图中展示的图像依次说明一下，最左边的是检测结果，中间的是提取的信号灯发光单元，最右边的图像是在使用颜色信息分割提取了红色和绿色区域后，使用canny算法提取的轮廓信息。

理想情况下，我们希望canny算法检测出来的信号灯发光单元的轮廓是连续完整的闭合轮廓线，这样的话就可以进行轮廓的凹凸性分析。但是从上面的实验结果看出，提取的信号灯轮廓线都是断开的。

但是我使用`findContours`函数找到canny边缘检测后得到的轮廓，发现找到的轮廓并没有断开（如下图右图所示），这个是个比较奇怪的问题，因为这样看的话，canny边缘检测得到的边缘并没有断开，可是为什么使用如下的代码直接显示`edge`会出现断开的问题呢？
![TLContours](http://7xniym.com1.z0.glb.clouddn.com/TLcontours.png)

{% highlight cpp %}
Canny(inputImg,edge,20,50,5);
imshow("edges", edge);
waitKey(1);
{% endhighlight %}
