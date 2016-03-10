---
layout:     post
title:      "在阿里的日子"
subtitle:   "summary of my internship in Alibaba" 
author:     "Jayn"
header-img: "/img/taogongzai.jpg"
tags:
  - 实习
---

# Alibaba实习工作总结
{:.no_toc}

&emsp;&emsp;在阿里实习了2个月的时间，这两个月里，自己在搜索事业部-研发工具与平台组进行Keludeopen的开发。总结一下自己两个月以来做的一些事情。

* 目录
{:toc}


### 实习期间完成的工作

* **帮助运营同学获取KO用户联系方式**
&emsp;&emsp;在完成这个任务的过程，多亏了宁哥的帮助。作为一个实习生，刚刚到KO，很多的业务都还很不熟悉。在做这个业务的过程中，自己还表示很好奇：咦？这些对象怎么都没有new一下，各种属性也没有指定，怎么声明一下就可以使用呢？然后了解到了这是spring框架，觉得好神奇，然后开始学习接触，了解了spring的3重注入方式，但是AOP切面编程还没来得及看，以后可以继续学习。然后接触了Maven,但是这个时候还不知道Maven的项目管理的强大功能。接触到了数据库，但是真的只是接触了一下，发现这边的程序获取的数据可以在数据库中保存更新，有点激动（突然觉得自己真是太low了。）在获取用户真实姓名时，使用了HSF服务，知道了如何引用HSF服务。但是这个时候还是有点晕，跌跌撞撞的算是完成了这个项目。现在想想，其实这个对于宁哥来说，真的是太easy了，但是我还是各种问，宁哥说的各种术语我也不清楚，比如UT,java bean，注入等等，估计都把宁哥问的无语了吧，自己也是太弱了，宁哥直接问了一句：你之前是写C++的是吧？他也只好拿这个理由来原谅我了吧？其实现在回过头来看，如果当时我能够在阿里技术协会ATA上能够看到关于实习生入职引导的文章，可以少走很多弯路，也不用经常去问一些低级的错误了。后期的话，宁哥走了，我主要跟飏风请教。他也给我很多的帮助，给我改了很多代码逻辑，指点了我如何在controller中写调用接口，在他的指导下，我写出了http接口，以及如何通过URL向接口中传入参数。并帮助我修改调试了很多spring注入的错误。
<br>    
* **完成KO的运营定时统计周报**
&emsp;&emsp;做完这个任务，我对数据库的SQL语句，Mybatis，Velocity从之前一点也不了解到开始了解，并可以上手完成项目需求了。虽然这里面深究还是有好多的东西我还没有深入了解。另外通过这个项目，我在调试过程中对spring注入配置有了更深刻的掌握。我从开始的出了错误一点头绪都没有，到可以看log，根据错误信息修改配置。当时觉得，将统计数据填入邮件模板中很复杂，因为根本不知道怎么搞，但是后来问清楚后，发现数据传入还是挺简单的，在这个过程中，对Velocity有了了解。现在来看，Velocity其实就是MVC的View层。了解了如何使用Mybatis进行数据库的操作，还记得当时因为resultMap,resultType的问题导致我查询的数据有问题，搞了一个下午。。。这些坑只有自己爬过，才会刻骨铭心。<br>

* **完成KO用户反馈的HSF接口和数据库设计,但最后还是将KO的用户反馈接入了先知平台**
&emsp;&emsp;关于KO用户反馈数据接入先知，使用的是先知提供的HSF接口。我向先知的剑儿，淏天询问相关的接入方法。这部分工作，我其实并不需要编写什么代码，只是去先知的网站上配置好，然后告诉前端相关的接入参数，其他的感觉前端使用先知的HSF接口使用方法跟使用KO自己的服务差不多，之前心旷调用的时候是无法调用的，后来萨波调用时也是出过问题。后来才明白，是他们的node端要配置ip，这样才可以调用成功。
&emsp;&emsp;而我写的KO自己的用户反馈的数据表与HSF接口，因为不采用这套方案，因此没有进行过测试。但是这个过程，让我更加了解，如何提供HSF服务。

* **完成KO泳道看板的数据库设计和HSF接口**
&emsp;&emsp;KO的泳道看板是项目的展示页面，每一个泳道相当于用户创建的一个项目分组，里面有用户创建的各个工作项，支持在泳道中的工作项的任意拖动。这个部分的话，主要要解决的问题就是泳道中的工作项来回拖动，要用一种什么样的形式来记录？选择的这种记录方式需要满足工作项序号因拖动导致的灵活变化。最后选择了字符串记录泳道内的工作项ID的方式解决。

---

### 在这个过程中我的一些收获
&emsp;&emsp;在这个过程中，我觉得自己的收获有以下一些方面：

+ **接触到一些之前没有接触过的技术**
在这边，KO使用的后端框架主要是：Spring MVC+Mybatis+Velocity
    - Spring MVC框架，我在实习中主要接触到是java bean注入，以及做周报定时发送任务时，使用到的定时设置。Spring MVC的注入使用的Ioc注入方式，在Spring的配置我觉得会比较麻烦，之后需要重新学习总结一下，除了注入，Spring MVC还有很多的内容我还没有深入学习，如controller等。关于Spring的定时任务，是使用Quatz来实现的。配置代码如下(实现的是每周四7点定时发送任务)：

```xml
<bean id="sendStatisticMessageBean"
        class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">
        <property name="targetObject" ref="sendStatisticMessageJob" />
        <property name="targetMethod" value="execute" />
    </bean>
    <bean id="sendStatisticMessageTriggerBean" class="org.springframework.scheduling.quartz.CronTriggerBean">
        <property name="jobDetail" ref="sendStatisticMessageBean" />
        <property name="cronExpression" value="0 0 7 ? *  THU" />
    </bean>
    
    <bean id="projectScheduledFactoryBean"
        class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
        <property name="triggers">
            <list>
                <ref bean="sendStatisticMessageTriggerBean"/>
            </list>
        </property>
    </bean>
```
   
   - Mybatis框架主要是跟数据库打交道，使用Mybatis可以操作后台数据库，将java代码和数据库操作链接到了一起，使用起来很方便。在映射文件中写好相关函数对应的的数据库操作后，在mybatis.xml中配置相应的映射文件即可。

下面的代码就是在映射文件Lane.xml中增加的数据库操作代码：

```xml
  <select id="getByUserIdAndProjectId" parameterType="Map" resultMap="lane">
        select
        <include refid="laneField" />
        from lane_ko
        where user_id = #{userId} and  project_id=#{projectId} and status != 9
    </select>
```

下面的代码是在Mybatis.xml中增加该映射文件的代码：

```xml
<mappers>
    <mapper resource="project/mapper/Lane.xml" />
</mappers>
```

+ Velocity的使用，主要是在做KO运营周报时使用到，运营周报统计了当前KO用户量，外部用户量，本周增加的用户量，活跃用户信息等数据。需要将这些数据以一种表格形式，用邮件发送给相关的人员。使用velocity模板制作了这个邮件模板。然后将统计的数据传给velocity模板，将数据填充到表格中。
+ Maven创建工程，添加依赖

+ **对java的一些实用更加深入**
    - 这包括java中接口的使用，java注解的学习了解（虽然没有自己写过，但是了解了其原理），java反射机制
+ **对于git的使用和团队合作开发的流程更加熟悉**
    - 包括gitlab的了解，git的pull,push,merge，以及分支等操作

---

### 我的一些感受
1. 团队的氛围还是不错的，人际关系简单，真诚
2. 团队的活动挺多，outing,light talk,新人秀等花样很多

----

### 遗憾的事情

* 阿里的内网有很多很好的学习资源，如ATA里面的很多技术博客都写得很不错。平常应该多去看看，学习交流一下的，可是我知道实习快要结束的时候，才发现了这篇新天地，很是遗憾。感觉仅仅是在写一些业务逻辑，其实是对自己的技术并没有太多的提高。
* 在实习的时候应该多问前辈。不懂就要问，我自己还是脸皮太薄，觉得总是问人家，会让别人很烦的。其实最后想想，还是自己想多了，他们人都是不错的。实习既要以提高自己的技术为主要目标，不应该想这么多没用的东西。
* 最遗憾的事情应该就是集团的拥抱变化，让我们无法转正了。在这边实习了两个月，对这边的生活还是很满意的，自己之前虽然也没有想着转正，但是身边地同是告诉我们基本上问题不大的，但是最后还是因为人才战略调整把我们这帮实习生坑了。不然的话，提前一年拿到转正offer是一件多么爽的事情。不过这也不一定是一件坏事，现在觉得公司一切都挺好的，不代表自己之后的想法会发生改变。就像阿里宣传的价值观，拥抱变化，我们自己也在不断变化中，不要这么早就把未来钉死。



