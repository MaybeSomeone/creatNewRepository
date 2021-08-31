# Runloop底层使用c语言编写  
> runloop中线程阻塞休眠时如何实现的?  
想要真正休眠线程只能通过内核层面的api来进行操作  


> 应用层api  
mach_msg()  

runloop是如何响应用户操作的？

runloop在实际开发中的应用  
控制线程生命周期（线程保活） 
解决NSTimer在滑动时停止工作的问题  
监控应用卡顿  
性能优化  


NSRunloopCommonModes 并不是一个真正的模式，他只是一个标记









































