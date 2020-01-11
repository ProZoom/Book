## Android自定义View

[TOC]

---

#### 基础

##### EventBus官方介绍

EventBus...

- simplifies the communication between components(简化了组件之间的通信)
    - decouples event senders and receivers(将事件发送者和接收者分离)
    - performs well with Activities, Fragments, and background threads(在Activity，Fragment和后台线程中表现良好)
    - avoids complex and error-prone dependencies and life cycle issues(避免复杂且容易出错的依赖关系和生命周期问题)
- makes your code simpler(使您的代码更简单)
- is fast(快)
- is tiny (~50k jar)(小)
- is proven in practice by apps with 100,000,000+ installs(已经通过100,000,000+安装的应用程序在实践中得到证实)
- has advanced features like delivery threads, subscriber priorities, etc.(具有交付线程，用户优先级等高级功能。)



##### 基本使用

EventBus是一种用于Android的事件发布-订阅总线，由GreenRobot开发。
它简化了应用程序内各个组件之间进行通信的复杂度，尤其是碎片之间进行通信的问题，可以避免由于使用广播通信而带来的诸多不便。

[Click Here To EventBus Github](https://github.com/greenrobot/EventBus)

###### 角色

- Event
- Subscriber
- Publisher

###### 线程模式

- POSTING
- MAIN
- BACKGROUND
- ASYNC

###### 引入依赖 

**Android Studio工程依赖**

[Click Here To EventBus Github](https://github.com/greenrobot/EventBus)


```groovy
implementation 'org.greenrobot:eventbus:3.1.1'
```

**定义事件**

**发布事件**

**黏性事件**

**优先级**



---

#### 进阶

##### 原理