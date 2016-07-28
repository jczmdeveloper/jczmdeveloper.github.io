
Fragment的生命周期和activity如何的一个关系

这我们引用本知识库里的一张图片： Mou icon

为什么在Service中创建子线程而不是Activity中

这是因为Activity很难对Thread进行控制，当Activity被销毁之后，就没有任何其它的办法可以再重新获取到之前创建的子线程的实例。而且在一个Activity中创建的子线程，另一个Activity无法对其进行操作。但是Service就不同了，所有的Activity都可以与Service进行关联，然后可以很方便地操作其中的方法，即使Activity被销毁了，之后只要重新与Service建立关联，就又能够获取到原有的Service中Binder的实例。因此，使用Service来处理后台任务，Activity就可以放心地finish，完全不需要担心无法对后台任务进行控制的情况。

Intent的使用方法，可以传递哪些数据类型。

通过查询Intent/Bundle的API文档，我们可以获知，Intent/Bundle支持传递基本类型的数据和基本类型的数组数据，以及String/CharSequence类型的数据和String/CharSequence类型的数组数据。而对于其它类型的数据貌似无能为力，其实不然，我们可以在Intent/Bundle的API中看到Intent/Bundle还可以传递Parcelable（包裹化，邮包）和Serializable（序列化）类型的数据，以及它们的数组/列表数据。

所以要让非基本类型和非String/CharSequence类型的数据通过Intent/Bundle来进行传输，我们就需要在数据类型中实现Parcelable接口或是Serializable接口。

http://blog.csdn.net/kkk0526/article/details/7214247

Fragment生命周期



注意和Activity的相比的区别,按照执行顺序

onAttach(),onDetach()
onCreateView(),onDestroyView()
Service的两种启动方法，有什么区别 1.在Context中通过public boolean bindService(Intent service,ServiceConnection conn,int flags) 方法来进行Service与Context的关联并启动，并且Service的生命周期依附于Context(不求同时同分同秒生！但求同时同分同秒屎！！)。

2.通过public ComponentName startService(Intent service)方法去启动一个Service，此时Service的生命周期与启动它的Context无关。

3.要注意的是，whatever，都需要在xml里注册你的Service，就像这样:

<service
        android:name=".packnameName.youServiceName"
        android:enabled="true" />
广播(Boardcast Receiver)的两种动态注册和静态注册有什么区别。

静态注册：在AndroidManifest.xml文件中进行注册，当App退出后，Receiver仍然可以接收到广播并且进行相应的处理
动态注册：在代码中动态注册，当App退出后，也就没办法再接受广播了
ContentProvider使用方法

http://blog.csdn.net/juetion/article/details/17481039

目前能否保证service不被杀死

Service设置成START_STICKY

kill 后会被重启（等待5秒左右），重传Intent，保持与重启前一样
提升service优先级

在AndroidManifest.xml文件中对于intent-filter可以通过android:priority = "1000"这个属性设置最高优先级，1000是最高值，如果数字越小则优先级越低，同时适用于广播。
【结论】目前看来，priority这个属性貌似只适用于broadcast，对于Service来说可能无效
提升service进程优先级

Android中的进程是托管的，当系统进程空间紧张的时候，会依照优先级自动进行进程的回收
当service运行在低内存的环境时，将会kill掉一些存在的进程。因此进程的优先级将会很重要，可以在startForeground()使用startForeground()将service放到前台状态。这样在低内存时被kill的几率会低一些。
【结论】如果在极度极度低内存的压力下，该service还是会被kill掉，并且不一定会restart()
onDestroy方法里重启service

service +broadcast 方式，就是当service走ondestory()的时候，发送一个自定义的广播，当收到广播的时候，重新启动service
也可以直接在onDestroy()里startService
【结论】当使用类似口口管家等第三方应用或是在setting里-应用-强制停止时，APP进程可能就直接被干掉了，onDestroy方法都进不来，所以还是无法保证
监听系统广播判断Service状态

通过系统的一些广播，比如：手机重启、界面唤醒、应用状态改变等等监听并捕获到，然后判断我们的Service是否还存活，别忘记加权限
【结论】这也能算是一种措施，不过感觉监听多了会导致Service很混乱，带来诸多不便
在JNI层,用C代码fork一个进程出来

这样产生的进程,会被系统认为是两个不同的进程.但是Android5.0之后可能不行
root之后放到system/app变成系统级应用

大招: 放一个像素在前台(手机QQ)

动画有哪两类，各有什么特点？三种动画的区别

tween 补间动画。通过指定View的初末状态和变化时间、方式，对View的内容完成一系列的图形变换来实现动画效果。 Alpha Scale Translate Rotate。

frame 帧动画 AnimationDrawable 控制 animation-list xml布局

PropertyAnimation 属性动画

Android的数据存储形式。

SQLite：SQLite是一个轻量级的数据库，支持基本的SQL语法，是常被采用的一种数据存储方式。 Android为此数据库提供了一个名为SQLiteDatabase的类，封装了一些操作数据库的api

SharedPreference： 除SQLite数据库外，另一种常用的数据存储方式，其本质就是一个xml文件，常用于存储较简单的参数设置。

File： 即常说的文件（I/O）存储方法，常用语存储大数量的数据，但是缺点是更新数据将是一件困难的事情。

ContentProvider: Android系统中能实现所有应用程序共享的一种数据存储方式，由于数据通常在各应用间的是互相私密的，所以此存储方式较少使用，但是其又是必不可少的一种存储方式。例如音频，视频，图片和通讯录，一般都可以采用此种方式进行存储。每个Content Provider都会对外提供一个公共的URI（包装成Uri对象），如果应用程序有数据需要共享时，就需要使用Content Provider为这些数据定义一个URI，然后其他的应用程序就通过Content Provider传入这个URI来对数据进行操作。

Sqlite的基本操作。

http://blog.csdn.net/zgljl2012/article/details/44769043

如何判断应用被强杀

在Applicatio中定义一个static常量，赋值为－1，在欢迎界面改为0，如果被强杀，application重新初始化，在父类Activity判断该常量的值。

应用被强杀如何解决

如果在每一个Activity的onCreate里判断是否被强杀，冗余了，封装到Activity的父类中，如果被强杀，跳转回主界面，如果没有被强杀，执行Activity的初始化操作，给主界面传递intent参数，主界面会调用onNewIntent方法，在onNewIntent跳转到欢迎页面，重新来一遍流程。

Json有什么优劣势。

怎样退出终止App

Asset目录与res目录的区别。

Android怎么加速启动Activity。

Android内存优化方法：ListView优化，及时关闭资源，图片缓存等等。

Android中弱引用与软引用的应用场景。

Bitmap的四种属性，与每种属性队形的大小。

View与View Group分类。自定义View过程：onMeasure()、onLayout()、onDraw()。

如何自定义控件：

自定义属性的声明和获取

分析需要的自定义属性
在res/values/attrs.xml定义声明
在layout文件中进行使用
在View的构造方法中进行获取
测量onMeasure
布局onLayout(ViewGroup)
绘制onDraw
onTouchEvent
onInterceptTouchEvent(ViewGroup)
状态的恢复与保存
Android长连接，怎么处理心跳机制。

View树绘制流程

下拉刷新实现原理

你用过什么框架，是否看过源码，是否知道底层原理。

Retrofit

EventBus

glide

Android5.0、6.0新特性。

Android5.0新特性：

MaterialDesign设计风格
支持多种设备
支持64位ART虚拟机
Android6.0新特性

大量漂亮流畅的动画
支持快速充电的切换
支持文件夹拖拽应用
相机新增专业模式
Android7.0新特性

分屏多任务
增强的Java8语言模式
夜间模式
Context区别

Activity和Service以及Application的Context是不一样的,Activity继承自ContextThemeWraper.其他的继承自ContextWrapper
每一个Activity和Service以及Application的Context都是一个新的ContextImpl对象
getApplication()用来获取Application实例的，但是这个方法只有在Activity和Service中才能调用的到。那么也许在绝大多数情况下我们都是在Activity或者Service中使用Application的，但是如果在一些其它的场景，比如BroadcastReceiver中也想获得Application的实例，这时就可以借助getApplicationContext()方法，getApplicationContext()比getApplication()方法的作用域会更广一些，任何一个Context的实例，只要调用getApplicationContext()方法都可以拿到我们的Application对象。
Activity在创建的时候会new一个ContextImpl对象并在attach方法中关联它，Application和Service也差不多。ContextWrapper的方法内部都是转调ContextImpl的方法
创建对话框传入Application的Context是不可以的
尽管Application、Activity、Service都有自己的ContextImpl，并且每个ContextImpl都有自己的mResources成员，但是由于它们的mResources成员都来自于唯一的ResourcesManager实例，所以它们看似不同的mResources其实都指向的是同一块内存
Context的数量等于Activity的个数 + Service的个数 + 1，这个1为Application
IntentService的使用场景与特点。

IntentService是Service的子类，是一个异步的，会自动停止的服务，很好解决了传统的Service中处理完耗时操作忘记停止并销毁Service的问题
优点：

一方面不需要自己去new Thread
另一方面不需要考虑在什么时候关闭该Service
onStartCommand中回调了onStart，onStart中通过mServiceHandler发送消息到该handler的handleMessage中去。最后handleMessage中回调onHandleIntent(intent)。

图片缓存

查看每个应用程序最高可用内存：

    int maxMemory = (int) (Runtime.getRuntime().maxMemory() / 1024);  
    Log.d("TAG", "Max memory is " + maxMemory + "KB");  
Gradle

构建工具、Groovy语法、Java

Jar包里面只有代码，aar里面不光有代码还包括

你是如何自学Android

首先是看书和看视频敲代码，然后看大牛的博客，做一些项目，向github提交代码，觉得自己API掌握的不错之后，开始看进阶的书，以及看源码，看完源码学习到一些思想，开始自己造轮子，开始想代码的提升，比如设计模式，架构，重构等。
