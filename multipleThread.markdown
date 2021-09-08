OSSpinLock 这是一种高级的锁

OSSPinLock 叫做自旋锁，等待锁的线程处于忙等状态，一直占用CPU资源，在之前一直是处于性能最高的锁，因为他不会让线程休眠。

自旋锁不安全，是因为可能会发生优先级反转问题
优先级高的线程会陷入忙等状态且不断分配cpu和时间，而挤占优先级较低的线程的时间让其无法执行，造成死锁问题。
static 实在编译阶段就可以确定的变量，不能进行函数调用

不是说有多线程 一定要加锁


互斥锁： 等不到锁就休眠,这是一种低级的锁 ll ll Lock，低级锁的特点就是睡觉



os_unfair_lock 从iOS10开始支持


递归锁 p_thread 是一种闲等，线程不会做事
p_thread_mutex 
pthread_mutex_lock(&_mutex)

NSLock NSRecursiveLock：基于p_thread_mutex做的封装
NSCondition 对mutex和Cond的封装















