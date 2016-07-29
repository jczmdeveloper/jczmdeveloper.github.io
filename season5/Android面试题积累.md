#阿里巴巴：
一面

•	说一下你怎么学习安卓的？

•	项目中遇到哪些问题，如何解决的？

•	Android事件分发机制？

•	三级缓存底层实现？

•	HashMap底层实现，hashCode如何对应bucket?

•	Java的垃圾回收机制，引用计数法两个对象互相引用如何解决？

•	用过的开源框架的源码分析

•	Acticity的生命周期，Activity异常退出该如何处理？

•	tcp和udp的区别，tcp如何保证可靠的，丢包如何处理？

二面：

•	标号1-n的n个人首尾相接，1到3报数，报到3的退出，求最后一个人的标号

•	给定一个字符串，求第一个不重复的字符 abbcad -> c

#美团：

一面

•	面向对象三大特性

•	Java虚拟机，垃圾回收

•	GSON

•	RxJava+Retrofit

•	图片缓存，三级缓存

•	Android启动模式

•	四大组件

•	Fragment生命周期，嵌套

•	AsyncTask机制

•	Handler机制

二面

•	面试官写程序，看错误。

•	面试官写程序让判断GC引用计数法循环引用会发生什么情况

•	Android进程间通信，Binder机制

•	Handler消息机制，postDelayed会造成线程阻塞吗？对内存有什么影响？

•	Debug和Release状态的不同

•	实现stack 的pop和push接口 要求：

o	1.用基本的数组实现

o	2.考虑范型

o	3.考虑下同步问题

o	4.考虑扩容问题


#新浪微博：

一面

静态内部类、内部类、匿名内部类，为什么内部类会持有外部类的引用？持有的引用是this？还是其它？

静态内部类：使用static修饰的内部类

匿名内部类：使用new生成的内部类

因为内部类的产生依赖于外部类，持有的引用是类名.this。

ArrayList和Vector的主要区别是什么？

ArrayList在Java1.2引入，用于替换Vector


Vector:

线程同步

当Vector中的元素超过它的初始大小时，Vector会将它的容量翻倍

ArrayList:

线程不同步，但性能很好

当ArrayList中的元素超过它的初始大小时，ArrayList只增加50%的大小

Java集合类框架

Java中try catch finally的执行顺序

先执行try中代码发生异常执行catch中代码，最后一定会执行finally中代码

switch是否能作用在byte上，是否能作用在long上，是否能作用在String上？

switch支持使用byte类型，不支持long类型，String支持在java1.7引入

Activity和Fragment生命周期有哪些？

Activity——onCreate->onStart->onResume->onPause->onStop->onDestroy

Fragment——onAttach->onCreate->onCreateView->onActivityCreated->onStart->onResume->onPause->onStop->onDestroyView->onDestroy->onDetach

onInterceptTouchEvent()和onTouchEvent()的区别？

onInterceptTouchEvent()用于拦截触摸事件

onTouchEvent()用于处理触摸事件

RemoteView在哪些功能中使用

APPwidget和Notification中

SurfaceView和View的区别是什么？

SurfaceView中采用了双缓存技术，在单独的线程中更新界面

View在UI线程中更新界面

讲一下android中进程的优先级？

前台进程
可见进程
服务进程
后台进程
空进程

tips：静态类持有Activity引用会导致内存泄露

二面

•	service生命周期，可以执行耗时操作吗？

•	JNI开发流程

•	Java线程池，线程同步

•	自己设计一个图片加载框架

•	自定义View相关方法

•	http ResponseCode

•	插件化，动态加载

•	性能优化，MAT

•	AsyncTask原理

•	65k限制

•	Serializable和Parcelable

•	文件和数据库哪个效率高

•	断点续传

•	WebView和JS

•	所使用的开源框架的实现原理，源码

#网易（杭州）：

一面：

•	Android中ClassLoader和java中有什么关系和区别？

•	熟不熟jvm，说一下Jvm的自动内存管理？

•	语言基础，String类可以被继承吗？为什么？

•	Final能修饰什么？（当时我说class、field、method，他说还有吗？然后又叫我不要在意，后来回想起，应该是问到我在参数里面要不要用final，接下来是因为匿名内部类）

•	Java中有内存泄露吗？（先说本质，再结合handler+匿名内部类）当时如何分析的？

•	描述下Aidl？觉得aidl有什么缺陷（这里在这个问题上回答有欠缺）

•	评价一下我，如果顺利进网易，需要往技术栈加什么点尽快投入业务？

二面：

•	用过什么开源，举一个例子？（volley）

•	Activity生命周期？情景：现在在一张act1点了新的act2，周期如何？

•	Act的launchMode，有没有结合项目用过（自己的程序锁和微信的PC端登陆对比，不过我现在又发现，应该大约估计可能是动态加载的一个缺陷，如果有找到相关信息，请务必跟我说。具体问题就是，当在PC端登录时，Android终端的微信会跳出，即使wechat的task不是在fore，当按下确认，返回的是wechat，而不是自己先前的app）

•	View的绘制原理，有没有用canvas自己画过ui？

•	以后想做Android什么方向？（中间件+SDK）

•	怎么看待前端和后端？

•	如果学前端会如何学？

•	优缺点？兴趣？

•	想不想来杭州？

•	评价一下我？往技术栈加什么？

三面HR：

•	为什么想来网易？

•	有投其他公司吗？

•	网易最吸引你的是什么？

•	想来杭州吗？

•	评价一下我？

豌豆荚：

豌豆荚一面

•	介绍一下你的项目

•	网络框架的搭建

•	图片加载框架的实现

•	写个图片浏览器，说出你的思路

•	上网站写代码，如下： 有一个容器类
ArrayList，保存整数类型的元素，现在要求编写一个帮助类，类内提供一个帮助函数，帮助函数的功能是删除 容器中<10的元素。

#豌豆荚二面

•	Activity的启动模式

•	事件分发机制

•	写代码，LeetCode上股票利益最大化问题

•	写代码，剑指offer上第一次只出现一次的字符

豌豆荚三面

•	写代码，反转字符串

•	写代码，字符串中出现最多的字符。


