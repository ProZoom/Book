[TOC]



### Java复习 

- 容器：
  * HashMap
  * HashSet
  * LinkedList：有一个面试官说，你能否自己写一个LinkedList，这里其实就是考察双向链表相关，比如加入数据，删除数据。如果不看源码，估计很难去知道内部原理。

- 内存模型

- 垃圾回收算法

```
JVM内存由几个部分组成：堆、方法区、栈、程序计数器、本地方法栈

JVM垃圾回收仅针对公共内存区域即：堆和方法区进行，因为只有这两个区域在运行时才能知道需要创建哪些对象，其内存分配和回收都是动态的。

- 类加载的过程
- 反射
- 多线程，线程池
```







### 算法与数据结构

比如常见的

- 单链表，反转，增删改查，
- 双链表，增删改查
- 常见的排序，堆排序，归并排序等。
- 二叉树的前序遍历，中序遍历，后序遍历等，最大K问题。经典的广度、深度优先搜索算法





------



### Android复习

- UI的自定义View

- 事件拦截和分发机制

- 解决过一些性能问题，以及项目中的实际应用。

-  性能优化工具：TraceView、Systrace、调试GPU过度绘制 & GPU呈现模式分析、Hierarchy Viewer、MAT、Memory Monitor & Heap Viewer & Allocation Tracker、LeakCanary、Lint。

- 性能优化方向：网络，内存，绘制，电量，APK瘦身

- IntentService原理及使用

- 缓存自己实现怎么做？LruCache原理

- 图形图像相关，如OpenGL ES管线流程，EGL的认识，Shader相关

- SurfaceView，TextureView，GlSurfaceView区别，使用场景？

- glide缓存策略？同一个图片跟size有关么
- android中的动画有哪些
- View事件传递机制
- 界面卡顿怎么排查和优化？
- Fragment的replace和end？？的区别？
- MVP，MVVM，MVC解释和实践
- 项目之外的，对技术的见解，拓展知识
- 微信跳一跳外挂怎么实现，检测怎么做的？

- 一张纯色背景下怎么有效检测各个矩形？
- 对接的so算法了解么，有接触过相关的库么？
- 三个算法题选一个并写出测试用例：打印n-m之间所有的素数；计算n-m之间1出现的次数；指定数字序列的排序；
- android api层的源码熟悉哪些？解释一下
- ACTION_CANCEL什么时候触发，触摸button然后滑动到外部抬起会触发点击事件吗，在+ + 滑动回去抬起会么
- 怎么处理嵌套View的滑动冲突问题
- 热修复相关的原理，框架熟悉么
- gradle打包流程熟悉么
- 任意提问环节：其实可以问之前面试中遇到的问题：比如，多模块开发的时候不同的负责人可能会引入重复资源，相同的字符串，相同的icon等但是文件名并不一样，怎样去重？

- Canvas的底层机制，绘制框架，硬件加速是什么原理，canvas lock的缓冲区是怎么回事
- surfaceview， suface，surfacetexure等相关的，以及底层原理
- android文件存储，各版本存储位置的权限控制的演进，外部存储，内部存储
- 上层业务activity和fragment的遇到什么坑？？页面展示上的一些坑和优化经验
- 网络请求的开源框架：OKHttp介绍，写过拦截器么

- 数据层有统一的管理么，数据缓存是怎么做的，http请求等有提供统一管理么？
- 有用什么模式么，逻辑什么的都在Activity层？怎么分离的
- 如果用了一些解耦的策略，怎么管理生命周期的？
- 有什么提高编译速度的方法？
- 对应用里的线程有做统一管理么？
- jni的算法提供都是主线程的？是不是想问服务类的啊
- 边沿检测用的啥？深度学习相关的有了解么？
- 上线后的app性能分析检测有做么

- 进程间通信方式？Binder的构成有几部分？
- HttpClient和HttpConnection的区别
- View的事件传递机制
- MVC，MVP，MVVM分别是什么？
- Android中常用的设计模式，说三个比较高级的？
- 内存优化，OOM的原因和排查方法
- 想改变listview的高度，怎么做
- Https是怎么回事？
- 除了日常开发，其他有做过什么工作？比如持续化集成，自动化测试等等

- IntentService和Service

```
生成一个默认的且与主线程互相独立的工作者线程来执行所有传送至onStartCommand() 方法的Intetnt。

生成一个工作队列来传送Intent对象给你的onHandleIntent()方法，同一时刻只传送一个Intent对象，这样一来，你就不必担心多线程的问题。在所有的请求(Intent)都被执行完以后会自动停止服务，所以，你不需要自己去调用stopSelf()方法来停止。

该服务提供了一个onBind()方法的默认实现，它返回null

提供了一个onStartCommand()方法的默认实现，它将Intent先传送至工作队列，然后从工作队列中每次取出一个传送至onHandleIntent()方法，在该方法中对Intent对相应的处理。

多次启动IntentService，IntentService是等前面的请求结束之后再执行下一个。 这个证实了 IntentService 采用单独的线程每次只从队列中拿出一个请求进行处理


这就是IntentService，一个方便我们处理业务流程的类，它是一个Service，但是比Service更智能。

```

- dp是什么，sp呢，有什么区别

  ```
  dp（dip）：device independent pixels，可以自动适配设备机型
  
  sp：适配字体的（会随着系统字体大小而改变，dp不会）
  
  px：像素点pixel px=dp*（dpi/160）
  
  dpi:dot per inch每英寸像素 dpi=px/inch（一般是对角线）
  
  ```

- 自定义View，ViewGroup注意那些回调？

  ```
  性能优化部分
  	onFinishInflate
  	    当系统解析XML中声明的View后回调此方法，调用顺序：内层			View->外层View,如果是viewgroup,适合在这里获取子View。
  		注意点：如果View没有在XML中声明而是直接在代码中构造的，则		不会回调此方法，此时无法获取到View的宽高和位置
  	onAttachedToWindow
  		当view 被添加到window中回调，调用顺序：外层View->内层			View。在XML中声明或在代码中构造，并调用addview（this 		view）方法都会回调该方法。
  	onDetachedFromWindow
  		看名字就知道是与void onAttachedToWindow()；对应的方			法，在VIew从Window中移除时回调，如执行removeView（）方		 法
  	onWindowFocusChanged
  		当View所在的Window获得或失去焦点时被回调此方法
  
  事件分发部分：
  	onInterceptTouchEvent
  	dispachTouchEvent
  	onTouchEvent
  	
  绘图部分：
  	onMeasure
  	onLayout
  	onDraw
  ```

  

- Android界面卡顿的原因以及解决方法

```
渲染机制：
    为了分析UI卡顿，我们有必要理解一下渲染机制，这套渲染机制适用于绝大部	   分的屏幕渲染，其中包括Android手机等众多屏幕设备。

    一些参数
    先来举个例子，电源胶卷时代播放的电影是24帧/秒，也就是说一秒有24张胶	片进行播放，这是早期的设定，比较低，因为交卷比较贵。随着科学技术的发		展，屏幕的刷新速度有了一个质的飞跃。

    渲染的一些重要参数：

    屏幕刷新理想的频率（硬件的角度）：60Hz
    理想的一秒内绘制的帧数，帧率（屏幕刷新的角度）：60fps
    这两个参数都是理想值，指代的都是同一个概念。实际情况中难免会比它们		低。在60fps内，系统会得到发送的VSYNC(垂直刷新/绘制)信号去进行		渲染，就会正常地绘制出我们需要的图形界面。Android手机进行绘制的		时候，GPU帮助我们将UI组件等计算成纹理Texture和三维图形			Polygons，同时会使用OpenGL---会将纹理和Polygons缓存在GPU内存里	面。

    其中，VSYNC：有两个概念

    Refresh Rate：屏幕在一秒时间内刷新屏幕的次数----有硬件的参数决		定，比如60HZ.
    Frame Rate：GPU在一秒内绘制操作的帧数，比如：60fps。
    基本结论
    要达到60fps，就要求：每一帧只能停留16ms。


根本原因：
    Android每隔16ms就会绘制一次Activity，如果由于一些原因导致了我们	 的逻辑、CPU耗时、GPU耗时大于16ms，UI就无法完成一次绘制，那么就会造	成卡顿。简单的一句话就是：卡主线程了。
    
    
解决方案：

一、外部因素引起的（以View为区分）

内存抖动的问题
方法太耗时了（CPU占用）
二、View本身的卡顿
	

GPU优化建议就是一句话：尽量避免过度绘制（overdraw）
如果我们的自定义控件存在一些被遮挡的不需要显示的区域，可以通过画布的裁剪来处理。例如下面的伪代码：

private void drawSomething(Canvas canvas , ...) {
    //画布的保存
    canvas.save();
    //裁剪画布
    canvas.clipRect(...);
    //绘制
    canvas.draw(...);
    //画布还原，下次继续使用
    canvas.restore();
}



```

- android中的存储类型

```
sharepreferrence
File
contentprovider
sqlite3
网络
```



- service用过么，基本调用方法

```
Service主要有两个应用场景：后台运行和跨进程访问

生命周期：
    onStart--->onStartCommand---->onDestory
    onBind

启动方式：
    startService
    bindService
```

- Handler机制

- LinearLayout、FrameLayout、RelativeLayout性能对比，为什么

- Activity的生命周期，finish调用后其他生命周期还会走么？
- FW层熟悉么，源码看过么
- GC回收机制熟悉么，分代算法知道么
- Java的类类加载原理
- 内存泄漏如何排查，MAT分析方法以及原理，各种泄漏的原因是什么比如
- Handler为什么会泄漏
- gradle熟悉么，自动打包知道么
- 介绍下先的app架构和通信
- 自己负责过哪些模块，跟同事相比自己的优势是什么
- 遇到过什么印象深刻的问题，怎么解决的

- 最近都做了哪些工作？
- 遇到了什么印象深刻的问题。A:会顺着你介绍的项目问下具体实现。
- 推送消息有富文本么？
- 热修复了解么，用的什么？
- apk包大小有限制么？怎么减少包大小？
- 工作中有没有用过或者写过什么工具？脚本，插件等等
- 比如：多人协同开发可能对一些相同资源都各自放了一份，有没有方法自动检测这种重复之类的
- 写过native的底层代码么
- view的绘制熟悉么，介绍下
- gc相关的算法
- anr是因为什么产生的，怎么排查
- 界面上的话，有什么优化措施么？比如列表展示之类的，平时遇到过内存问题吗，怎么优化的？
- 平时用过哪些设计模式？

- 介绍下最近一年主要做了什么工作
- 会对简历上突出的技能进行详情的询问：
  比如：音频合成的具体步骤，以及遇到的一些问题和细节处理。
  会根据面试发散一些问题，问到，seek方法播放到末尾后重新播放会有一些卡顿的不流畅问题，怎么避免，从交互设计或者技术角度。（个人表示没怎么关注这种）。
- 项目团队多少人，怎么分配工作
- 线程之间怎么通信的？
- app的架构是怎么样的，并且为什么这样，有什么优缺点？
- 算法熟悉么？给了一个二叉排序树，出了一个给定节点找到它的下一个元素（指的是大小顺序的下一个）的算法题。
- 为什么找工作，自己的优势是什么

- 技术问题不再局限于简历，可能根据简历和回答情况渐进并扩散。
- 感觉各个技术面试官之前并没有沟通，可能会问到类似的问题
- 介绍下自己主要负责的工作
- Activity的生命周期有哪些，知道onRestart么，介绍下
- savedInstanceState知道么，干什么用的，什么时候有值，什么时候为空，平时是怎么用的
- View绘制熟悉么，介绍下，能说下是实现原理么？
- 平时用过什么开发工具，分析工具？
- ANR是怎么回事？怎么查？Service会引起ANR么？
- Activity的启动模式有哪些？栈里是A-B-C，先想直接到A，BC都清理掉，有几种方法可以做到？这几种方法产生的结果是有几个A的实例？
- 有什么工具可以看到Activity栈信息么？多个栈话，有方法分别得到各个栈的Activity列表么
- 都熟悉哪些命令？知道怎么用命令启动一个Activity么?
- SharedPrefrences的apply和commit有什么区别
- java里带$的函数见过么，是什么意思
- MD5是加密方法么，Base64呢
- 有博客和github，主要是写的什么？有哪些关注
- android 8.0 有哪些新特性
- 差不多就这些吧。。最后每个面试官都会让你问他问题。

- ActivityA跳转ActivityB然后B按back返回A，各自的生命周期顺序，A与B均不透明。
- Synchronize关键字后面跟类或者对象有什么不同。
- 单例的DCL方式下，那个单例的私有变量要不要加volatile关键字，这个关键字有什么用
- JVM的引用树，什么变量能作为GCRoot？GC垃圾回收的几种方法
- ThreadLocal是什么？Looper中的消息死循环为什么没有ANR？
- Android中main方法入口在哪里
- jdk1.5？SparseArray和ArrayMap各自的数据结构，前者的查找是怎么实现的，与HashMap的区别
- Runnable与Callable、Future、FutureTask的区别，AsyncTask用到哪个？- AsyncTask是顺序执行么，for循环中执行200次new AsyncTask并execute，会有异常吗
- IntentService生命周期是怎样的，使用场合等
- RecyclerView和ListView有什么区别？局部刷新？前者使用时多重type场景下怎么避免滑动卡顿。懒加载怎么实现，怎么优化滑动体验。
- SQLite的数据库升级用过么
- 开放问题：如果提高启动速度，设计一个延迟加载框架或者sdk的方法和注意的问题。
- Scroller有什么方法，怎么使用的。
- 分享下项目中遇到的问题
- webwiew了解？怎么实现和javascript的通信？相互双方的通信。@JavascriptInterface在？版本有bug，除了这个还有其他调用android方法的方案吗？
- ReactiveNative了解多少
- JNI和NDK熟悉么？Java和C方法之前的相互调用怎么做？







----

------

### Android Framework复习

- AMS管理

- Activity启动流程

-  Binder机制

- AIDL原理

-  为什么用Parcelable，好处是什么？

- Android图像显示相关流程，VSync信号，SurfaceFlinger到FrameBuffer





------



### 第三方框架

- Glide源码介绍

- OKHttp源码介绍
- RXJava源码介绍

- EventBus源码原理

- LeakCanary源码原理

- ARouter源码原理

- 插件化相关框架，来不及细看，了解不同插件机制主要原理和流派，还有优缺点和局限性。



------



---











----


