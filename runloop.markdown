# Runloop

> runloop的概念： 运行循环，在程序运行过程中循环做一些事情
 
 应用范轴
 1.定时器
 2.PerformSelector
 3.GCD Async Main Queue
 4.时间响应 手势识别 界面刷新
 5.网络请求
 6.AutoReleasePool
 
 
 如果有了runloop?
 程序不会马上退出，而是保持运行状态
 
 runloop的基本作用
 保持程序的持续运行
 处理app的各种事件
 处理app中的各种事件 比如触摸事件 比如定时器事件 
 节省cpu资源，提高程序性能，该做事时做事，该休息时休息
 
 UIApplicationMain 该函数会创建一个runloop对象
 
 int retVal = 0;
 do {
  sleep_and_wait()
 } while(retVal === 0)
 
 
## iOS有两套API来访问和使用Runloop。
*1.NSRunloop
2.CFRunloop*
> NSRunloop 基于CFRunloop的包装，NS类型都是基于对CF类型的包装。

runloop对象存储在全局的字典中
thread是key, runloop是value
线程结束时，runloop也会销毁
主线程runloop自动创建或获取，子线程默认没有开启runloop

__CFRunloopMode *CFRunloopModeRef 
如果需要切换mode，会在loop内部进行切换，而不需要退出整个loop重新开始一个新的loop


## 常见的两种Mode
kCFRunloopDefaultMode app的默认mode,通常主线程是在这个mode下运行
**NSDefaultRunLoopMode**
**UNTrackingRunLoopMode**
**注意**：切换mode不会导致runloop的退出，mode的切换也是在内部完成切换。

## NSRunloopModeRef 内部数据

**Source0** 触摸事件的处理
**Source1** 基于port的线程间通信
系统时间捕捉

**Timer**
NSTimer
perforSelector: withObject: afterDelay


**ObServers**
用于监听Runloop的状态
UI刷新 BeforeWaiting
AutoRealese pool BeforeWaiting

kCFRunLoopModes 默认包括两种模式

Runloop决定代码什么时候执行







 
 
 
 
 
 
 


