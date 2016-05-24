---
layout:     post
title:      "RunLoop"
subtitle:   ""
date:       2015-10-20
author:     "Bin"
header-img: "img/home-bg-o.jpg"
tags:
    - iOS
    
---

什么是RunLoop？
在OC这门面向对象编程的语言中，RunLoop也可以当做一个对象，其功能是保持程序的持续运行，处理App中的各种事件
（比如触摸事件、定时器事件、Selector事件）节省CPU资源，提高程序性能，底层是个do-while 循环。

RunLoop与线程
- 每条线程都有唯一的一个与之对应的RunLoop对象，用字典的key和value一一对应。
- 主线程的RunLoop由系统自动创建并启动，子线程的RunLoop需要主动创建并启动。
- RunLoop在第一次获取时创建，在线程结束时销毁

RunLoop对象
- CFRunLoopRef（c） 是在 CoreFoundation 框架内的，它提供了纯 C 函数的 API，所有这些 API 都是线程安全的。
-- CFRunLoopGetCurrent(); // 获得当前线程的RunLoop对象
-- CFRunLoopGetMain(); // 获得主线程的RunLoop对象

- RunLoop 有5个类:
-- CFRunLoopRef
-- CFRunLoopModeRef  //切换Mode，只能退出Loop，再重新指定一个Mode进入

系统默认注册了5个Mode:
kCFRunLoopDefaultMode：App的默认Mode，通常主线程是在这个Mode下运行 （将图像等处理在此模式下运行）
UITrackingRunLoopMode：界面跟踪 Mode，用于 ScrollView 追踪触摸滑动，保证界面滑动时不受其他 Mode 影响（应用滑动式停止其他图像处理，提高性能及用户体验）
UIInitializationRunLoopMode: 在刚启动 App 时第进入的第一个 Mode，启动完成后就不再使用
GSEventReceiveRunLoopMode: 接受系统事件的内部 Mode，通常用不到
kCFRunLoopCommonModes: 这是一个占位用的Mode，不是一种真正的Mode（应用：commonModes  兼容Default和Tracking模式，当 RunLoop 的内容发生变化时，RunLoop 都会自动将 _commonModeItems 里的 Source/Observer/Timer 同步到具有 "Common" 标记的所有Mode里）

-- CFRunLoopSourceRef  //事件源
Source0：非基于Port的
Source1：基于Port的，通过内核与其他线程通信，分发任务 （应用：利用port 保证线程的常驻）

-- CFRunLoopTimerRef
是基于时间的触发器NSTimer，在runloop内时间精确不够，有误差，用GCD dispatch_source_t，


-- CFRunLoopObserverRef
（应用）观察者，能够监听RunLoop的状态改变
kCFRunLoopEntry         = (1UL << 0), // 即将进入Loop （主线程 RunLoop 里注册了两个Observer，第一个监听entry，最高优先级，保证在回调之前创建autoreleasepool）
kCFRunLoopBeforeTimers  = (1UL << 1), // 即将处理 Timer
kCFRunLoopBeforeSources = (1UL << 2), // 即将处理 Source
kCFRunLoopBeforeWaiting = (1UL << 5), // 即将进入休眠  （第二个Observer监听，最低优先级，autoreleasepool释放并创建autoreleasepool）
kCFRunLoopAfterWaiting  = (1UL << 6), // 刚从休眠中唤醒
kCFRunLoopExit          = (1UL << 7), // 即将退出Loop （（第二个Observer监听，最低优先级，保证释放在所有回调之后，autoreleasepool释放autoreleasepool）

Source/Timer/Observer 被统称为 mode item，一个 item 可以被同时加入多个 mode。但一个 item 被重复加入同一个 mode 时是不会有效果的。如果一个 mode 中一个 item 都没有，则 RunLoop 会直接退出，不进入循环。
-开启子线程runloop 应用@autoreleasepool { 子线程内也有很多小的释放池  }
-connection 内有runloop，放子线程必须启动子线程的runloop，否则无法执行
-要手动停止runloop 要用CFRunLoop，并用属性获取当前runloop，
-time不准，用GCD dispatch_source_t,其实OC对象 ，应强引用，不让创建完就销毁了，GCD默认暂停，要dispatch_resume、dispatch_cancel 并清空强引用。

CFRunLoop对外暴露的管理 Mode 接口只有下面2个:
CFRunLoopAddCommonMode(CFRunLoopRef runloop, CFStringRef modeName);
CFRunLoopRunInMode(CFStringRef modeName, ...);

Mode 暴露的管理 mode item 的接口有下面几个：
CFRunLoopAddSource(CFRunLoopRef rl, CFRunLoopSourceRef source, CFStringRef modeName);
CFRunLoopAddObserver(CFRunLoopRef rl, CFRunLoopObserverRef observer, CFStringRef modeName);
CFRunLoopAddTimer(CFRunLoopRef rl, CFRunLoopTimerRef timer, CFStringRef mode);
CFRunLoopRemoveSource(CFRunLoopRef rl, CFRunLoopSourceRef source, CFStringRef modeName);
CFRunLoopRemoveObserver(CFRunLoopRef rl, CFRunLoopObserverRef observer, CFStringRef modeName);
CFRunLoopRemoveTimer(CFRunLoopRef rl, CFRunLoopTimerRef timer, CFStringRef mode);


NSRunLoop（oc） 是基于 CFRunLoopRef 的封装，提供了面向对象的 API，但是这些 API 不是线程安全的。
[NSRunLoop currentRunLoop]; // 获得当前线程的RunLoop对象
[NSRunLoop mainRunLoop]; // 获得主线程的RunLoop对象


