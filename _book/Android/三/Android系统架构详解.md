## Android系统架构详解

Android的架构是分层的，非常清晰，分工很明确。
Android本身是一套软件堆叠(Software Stack)，或称为「软件叠层架构」，
叠层主要分成三层：操作系统、中间件、应用程序。
Android系统分为4层，包括：


![fuck.png](https://i.loli.net/2018/04/04/5ac48d2aef2af.png)

![image001.png](https://i.loli.net/2018/05/23/5b04c5f528ca7.png)



- Linux内核
    
    Android的核心系统服务依赖于Linux内核，如
    
    - 安全性
    - 内存管理
    - 进程管理
    - 网络协议栈
    - 驱动模型
  
    Linux 内核也同时作为硬件和软件栈之间的抽象层
    
    驱动：
    - 显示驱动（Display Driver）
    - 摄像头驱动（Camera Driver）
    - 键盘驱动（Keypad Driver）
    - 蓝牙驱动（Bluetooth Driver）
    - WiFi驱动（WIFI Driver）
    - Audio驱动（Audio Driver）
    - Binder（IPC）驱动
    - 电源管理（Power Manager）
    - M-Systems驱动
    
- 系统库（Libraries）

    Android包含一个C/C++库的集合，供Android系统的各个组件使用。这些功能通过Android的应用程序框架（application framework）暴露给开发者。下面列出一些核心库：
    
    - 界面管理（Surface Manager）
    - 多媒体框架（Media Framework）
    - SQLite
    - 字体引擎（FreeType）
    - 图形处理3D库（OpenGL|ES）
    - SGL
    - 浏览器（WebKit）
    - 安全套接层（SSL）
    - C库


- Android运行环境（Android Runtime）
	
	- Core Libraries 
	- Dalvik Virtual Machine 
	
    Core Libraries 提供大部分在Java编程语言核心类库中可用的功能


    Dalvik Virtual Machine，每一个Android应用程序是Dalvik虚拟机中的实例，运行在他们自己 的进程中。Dalvik虚拟机设计成，在一个设备可以高效地运行多个虚拟机。Dalvik虚拟机可执行文件格式是.dex，dex格式是专为Dalvik 设计的一种压缩格式，适合内存和处理器速度有限的系统。大多数虚拟机包括JVM都是基于栈的，而Dalvik虚拟机则是基于寄存器的。 两种架构各有优劣，一般而言，基于栈的机器需要更多指令，而基于寄存器的机器指令更大。dx 是一套工具，可以將 Java .class 转换成 .dex 格式。一个dex文件通常会有多个.class。由于dex有時必须进行最佳化，会使文件大小增加1-4倍，以ODEX结尾。Dalvik虚拟机依赖于Linux 内核提供基本功能，如线程和底层内存管理。
	  

- 应用程序框架层（Application Framework）

	Android的应用程序框架为应用程序层的开发者提供APIs，它实际上是一个应用程序的框架,应用程序的体系结构旨在简化组件的重用，任何应用程序都能发布他的功能且任何其他应用程序可以使用这些功能（需要服从框架执行的安全限制）。这一机制允许用户替换组件。。由于上层的应用程序是以JAVA构建的，因此本层次提供的首先包含了UI程序中所需要的各种控件：例如： Views (视图组件)包括 lists(列表), grids(栅格), text boxes(文本框), buttons(按钮)等。甚至一个嵌入式的Web浏览器。

	- [活动管理器(Activity Manager Services)](\framework\ActivityManager.md)
	- [窗口管理器（Window Manager Services）]()
	- [内容提供者（Content Providers）]()
	- [视图系统（View Systems）]()
	- [通知管理器（Notification Manager）]()
	- [包管理器（Package Manager）]()
	- [电话管理器（Telephony Manager）]()
	- [资源管理器（Resource Manager）]()
	- [定位管理层（Location Manager）]()
	- [XMPP Service]()


- 应用程序层

Android的应用程序主要是用户界面（User Interface）方面的，通常以JAVA程序编写，其中还可以包含各种资源文件（放置在res目录中）JAVA程序及相关资源经过编译后，将生成一个APK包。
Android本身提供了主屏幕（Home），联系人（Contact），电话（Phone），浏览器（Browers）等众多的核心应用。
同时应用程序的开发者还可以使用应用程序框架层的API实现自己的程序。这也是Android开源的巨大潜力的体现。



### 系统服务

SystemServer 是framework中非常重要的一个进程，它是在虚拟机启动后运行的第一个java进程，SystemServer启动其他系统服务，这些系统服务都是以一个线程的方式存在于SystemServer进程中。 

- EntropyService	提供伪随机数

- PowerManagerService	电源管理服务

- ActivityManagerService	最核心的服务之一，管理Activity

- TelephonyRegistry	通过该服务注册电话模块的事件响应，比如重启、关闭、启动等

- PackageManagerService	程序包管理服务

- AccountManagerService	账户管理服务，是指联系人账户，而不是Linux系统的账户 

- ContentService	ContentProvider服务，提供跨进程数据交换

- BatteryService	电池管理服务

- LightsService	自然光强度感应传感器服务

- VibratorService	震动器服务 

- AlarmManagerService	定时器管理服务，提供定时提醒服务

- WindowManagerService	Framework最核心的服务之一，负责窗口管理

- BluetoothService	蓝牙服务

- DevicePolicyManagerService	提供一些系统级别的设置及属性

- StatusBarManagerService	状态栏管理服务

- ClipboardService	系统剪切板服务

- InputMethodManagerService	输入法管理服务

- NetStatService	网络状态服务

- NetworkManagementService	网络管理服务

- ConnectivityService	网络连接管理服务

- AccessibilityManagerService	辅助管理程序截获所有的用户输入，并根据这些输入给用户一些额外的反馈，起到辅助的效果

- MountService	挂载服务，可通过该服务调用Linux层面的mount程序

- NotificationManagerService	通知栏管理服务，Android中的通知栏和状态栏在一起，只是界面上前者在左边，后者在右边

- DeviceStorageMonitorService	磁盘空间状态检测服务

- LocationManagerService	地理位置服务

- SearchManagerService	搜索管理服务

- DropBoxManagerService	通过该服务访问Linux层面的Dropbox程序

- WallpaperManagerService	墙纸管理服务，墙纸不等同于桌面背景，在View系统内部，墙纸可以作为任何窗口的背景 

- AudioService	音频管理服务

- BackupManagerService	系统备份服务

- AppWidgetService	        Widget服务

- RecognitionManagerService	身份识别服务

- DiskStatsService	磁盘统计服务


