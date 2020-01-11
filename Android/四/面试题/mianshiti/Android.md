## Activity
* Activity是什么

    Activity是Android组件中最基本也是最为常见的四大组件之一（Activity、Service、ContentProvider、BroadCastReceiver）

* 生命周期

    onCreate-->onStart-->onResume--> (activity is running) -->onPause-->onStop-->onDestory
				|															|
				|--------------<---------onRestart----------<---------------|
	
* 启动模式

    启动模式简单地说就是Activity启动时的策略，在AndroidManifest.xml中的标签的android:launchMode属性设置；启动模式分别分为以下四种：
	 1.standard（）
		每次激活Activity时(startActivity)，都创建Activity实例，并放入任务栈；
     2.singleTop (为什么要用signleTop而不是直接跳转的时候判断一下即可)
		如果某个Activity自己激活自己，即任务栈栈顶就是该Activity，则不需要创建，其余情况都要创建Activity实例；
		actvity的生命周期是通过跨进程，先走ActivityManagerService查询状态，后返回到App所在ActivityThread那里再处理。AMS不在同一进程的，无法修改规则。

     3.singleTask
		如果要激活的那个Activity在任务栈中存在该实例，则不需要创建，只需要把此Activity放入栈顶，并把该Activity以上的Activity实例都pop；
     4.singleInstance
		如果应用1的任务栈中创建了MainActivity实例，如果应用2也要激活MainActivity，则不需要创建，两应用共享该Activity实例；
	
	任务栈：每个应用都有一个任务栈，是用来存放Activity的，功能类似于函数调用的栈，先后顺序代表了Activity的出现顺序；

* 通讯方式

    1. Intent (传递值的大小，1M左右)
	2. startActivityForResult
	3. LocalBroadCastManger
	4. sendBroadCast
	5. EventBus
	
* 状态保存/恢复，以及调用时机

	1. onSvaeInstanceState
	2. onRestoreInstanceState

* 避免Activity重新创建的方法

	1. 禁止屏幕旋转
	
* Activity的Task

* ANR时长

	1.Activity
	
	2.BroadCastReceiver
		前台广播默认10秒，后台广播默认60秒---->ActivityManagerService.java里的BROADCAST_FG_TIMEOUT、BROADCAST_BG_TIMEOUT 

	3.Service
		前台Service20秒，后台服务200秒----->ActivityServices.java里的SERVICE_TIMEOUT、SERVICE_BACKGROUND_TIMEOUT

	4.按键
		默认5秒，---->ActivityManagerService.java里的KEY_DISPATCHING_TIMEOUT


---------

## Fragment
* 生命周期
	1. onAttach
	1. onCreateView
	1. onActivityCreate
	1. onStart
	1. onReusme
	1. onPuase
	1. onStop
	1. onDestroyView
	1. onDestroy
	1. onDetach
* Fragment与Fragment通讯方式
* Fragment与Activity通讯方式
	1. 通过getActivity强转成宿主Activity调用其方法
	1. 
* 回退栈
* add与replace区别
	1. add
	1. replace
* 创建方式
	1. 静态创建
	1. 动态创建
* 销毁与重建
	1. 当Activity被以外销毁的时候，所包含的Fragment也会被销毁，但也会调用Fragment的onSaveInstanceState进行缓存，并且保留之前的setArguments的Bundle

-----------------------------



## BroadcastReceiver
* BroadCastReceiver生命周期
* 无序广播
* 有序广播
* 粘滞广播
* 系统广播种类
* 广播优先级
* 动态注册
* 静态注册
* ANR时长
* 能否BindService
	1. 不可，但可startService
	1. BindService必须是Activity的Context

## Service
* startService生命周期
* bindService生命周期
* startService+bindService生命周期
* 启动方式
* Service类型
	1. 按地点分类
		1. 本地Service
		1. 远程Service
	1. 按运行分类
		1. 前台Service
		1. 后台Service
	1. 按功能分类
		1. 可通讯Service
		1. 不可通讯Service
* 自动销毁Service
	1. startService不会自动销毁，调用者销毁后还在
	1. bindService会自动销毁，随着调用者(Activity)销毁而销毁
	1. startService+bindService，调用者销毁后还在，但会调用onUnBind()，需要手动stopService才能销毁
* ANR时长

## Context
* 什么是Context
	1. 它描述的是一个应用程序环境的信息,即上下文
	2. 该类是一个抽象(abstract class)类,Android 提供了该抽象类的具体实 现类(ContextIml)
	3.通过它我们可以获取应用程序的资源和类,也包括一些应用级别操作, 例如:启动一个 Activity,发送广播,接受 Intent,信息,等

## Context, Activity,Appliction的区别
	1. Context是程序的上下文，通过它可以获取程序中各种资源
	2. Activity和Application都是Context的子类
	3. Activity和Application维护的的生命周期不一

## Intent https://blog.csdn.net/harvic880925/article/details/38399723
* IntentFilter
	1. Action
	1. Category
	1. Data
* 传递数据类型

## URI / URL
* 什么是URI
* 什么是URL

## SharedPerferences
* 安全性
* 优化

## Serializable / Parcelable 
* Serializable作用
* Serializable优点
* Parcelable作用
* Parcelable优点

## 子线程与主线程交互方式和优缺点

## Bitmap
* 压缩
* 加载大图策略

## 三级缓存

## LruCache、DiskLruCache

## View体系
* View生命周期
* View事件
* 刷新机制
* View绘制流程
	1. onLayout
	1. onMeasure
	1. onDraw
* requestLayou、invalidate、postInvalidate区别
	1. requestLayout
	1. invalidate
	1. postInvalidate
* 更新UI的方式
	1. Handler
	1. runOnUiThread
		原理很简单，就是传入一个Runnalbe，Activity会判断当前是否在主线程，如果是，
		直接调用Runnable.run，直接调用run不是启动线程这点都知道吧，如果不是主线程
		则调用Activity内部持有的主线程的Handler发送一个Post，这个POST也不是开启线程的，
		熟悉Handler的应该知道，这篇文档也会说Handler
	1. view.post
	1. 特殊情况子线程能更新UI，子线程更新UI时候会经过ViewRootImpl的checkThread判断是否是主线程，
	   但是这个ViewRootImpl是在OnResume的时候才创建出来的，所以在onCreate到onResume之前是可以有时间在子线程更新UI的


* 辅助工具
	1. Scoller
	
## RelativeLayout、LinearLayout、FrameLayout
* 作用
* 优缺点

* RecyclerView、ListView
* RecyclerView缓存机制
* ListView缓存机制
* 为什么会图片错位

## WebView https://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650825278&idx=1&sn=312f61cac2d4a2839e7457fa7e77fe76&chksm=80b7b6a0b7c03fb6f528fc23825a98a1ab33945d479eab5dea8a5464350daf2c5efd49ad1e62&mpshare=1&scene=1&srcid=0412t0eUjki3R7DgS9snTadF#rd
* JS交互
* WebView隐患
* 优化WebView

## NotifiCationCompat

## 动画
* 帧动画
* 补间动画
* 属性动画

## MediaPlyear

## Handler
* Looper
* Message Queue
* ThreadLocal
* Message
* Handler

## AsycTask

## Binder
* 序列化与反序列化的过程

## OKHttp
* 源码

## Rx
* 源码

## Retrofit
* 源码

## HTTP协议
* 请求过程
* TCP与UDP的区别。
* TCP和UDP报文结构。
* TCP的三次握手与四次挥手过程。
* TCP可靠传输原理实现（滑动窗口）。
* TCP拥塞控制。
* TCP流量控制。
* Http的报文结构。
* Http的状态码。
* Http的请求方法。
* Http1.1和Http1.0及2.0的区别
* Http长连接。
* Cookie、Session、TOKEN的作用和原理。https://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650825233&idx=1&sn=5ec8c8285771deaf9278f30cda6b21c3&chksm=80b7b68fb7c03f997e6723e3dce4fb6746d2ff157c0281fd3a22ac4c6e01100e5fc8f9c7eca8&mpshare=1&scene=1&srcid=0412IXGvWvDkcy5z357Bz0Ss#rd
* Https加密原理。

## 5.0适配

## 6.0适配

## 7.0适配

## 8.0适配

## 9.0适配

## 屏幕适配

## 资源适配

## 内存泄漏、内存溢出 http://mp.weixin.qq.com/s/8bQ4ES5SBvnvPoaw8aGcig
* 内存泄漏引发的问题
	1. 可能使程序造成卡顿的现象，或者莫名的消失，因为内存过大，系统就更可能的回收这一块的内存，或者直接崩溃
* 内存泄漏的原因
	1. handler等生命周期较长的匿名内部类，因为这些匿名内部类可能会持有外部的引用，
	   从而导致短期内就算Activity退出而一些资源没有被回收，数据结构未优化，图片没有优化，
	   没有注意到对象的生命周期，造成许多对象没有被回收，过多的使用Service，单例的过多使用，无效的资源等等

## 内存优化

## 卡顿优化

## 电量优化

## APK体积优化

## 各种SDK操作策略
	1. 推送
		1. APP关闭后推送策略
	1. IM通讯
		1. 记录存取
	1. 统计
		1. 自定义埋点
	1. 地图
		1. 启动定位、关闭定位
		1. 保存上一次请求作为下一次初始值
	1. 支付
		1. 加密
		1. 回调

## AseetManager
## AseetManager

## ActivityManager

## SystemManager

## WindowManager

## PackageManager

## ResoureceManager

## ClassLoader
* BootstrapClassLoader
	1. 加载路径
* ExtClassLoader
	1. 加载路径
* AppClassLoader
	1. 加载路径
* PathClassLoader
	1. 加载路径
* DexClassLoader
	1. 加载路径

## App的启动过程

## GC

## 线程

## 线程池

## adb命令

## Gradle命令

## 设计模式

## 算法

## WIFI机制

## 蓝牙机制