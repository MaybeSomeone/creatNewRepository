# runtime 方法查找

if (receiver) {
  Cache cache = self.cache;
  IMP localImp = searchForLocalCache(cache);
  if (!localImp) {
     IMP rwImp = searchFromClass_rw_t();
     if (!rwImp) {
        var notFound = true;
        while(Class.superClass && notFound);
        Cache superClassCache = Class.superClass().Cache;
        IMP superClassImp = searchForLocalCache(superClassCache);
        if (superClassImp) {
           notFound = false
           return superClassImp;
        }
        goto dynamicResolver;
     }
  } else {
    return localImp;
  }
}

struct method_t {
  SEL sel;
  char \*types;
  IMP imp;
}

动态添加方法之后会重新进入消息转发流程，再次从缓存和方法列表搜索方法是否存在。



if (已经动态解析过： triedResolver) {
  消息发送
} else {
  进行动态解析
}



// 消息转发机制

// runtime 关联对象的实现，变量存储位置 ，以及他的数据结构

// runloop的具体使用场景

// 进程与线程的区别

// 加密算法有哪一些算法以及实现

// 那些线程锁原理以及使用场景
  互斥锁的实现和原理
  
 // react 当我想阻止一个方法向下传递该如何处理
 
 // react 当dom中有大量更新时有多少react做了哪些事情来提升性能
 
 // category和extension为什么重写父类不适合用category
 
 // react 基于redux-saga 有哪些单元测试框架可以使用
 
 // 算法



1.消息发送流程
2.动态解析流程
3.消息转发流程


// ------------------------------消息发送流程-----------------------------------
方法一旦调用则直接objc_MsgSend(reciever, @selector(test));
reciever ！= nil -> isa -> cache -> class_rw_t -> method_t ->return implementation

isa -> superClass -> cache-> class_rw_t -> methood -> return implementation

// ------------------------------动态解析流程-----------------------------------
动态解析流程只会进入一次，返回对象之后重新进入消息发送流程，如果还是没有找到对应对象来处理则进入消息转发流程
+（)resolveInstanceMethod()
+ ()resolveClassMethod()


// ------------------------------消息转发流程-----------------------------------
- (id)forwardingTargetForSelector() {
  
}

// 如果该方法返回值为空，则直接报错，不为空则直接重新进入消息转发流程
- (NSMethodSiganiture *)methodSignatureForSelector:(SEL)aSelector { 
    if (aSelector == @selector(test)) {
      return nil;
    }
    return [super methodSignatureForSelector: aSelector];
}

// anInvocatin就是方法调用者 包括方法调用者 方法名 方法参数
// anInvocation.target 方法调用者
// anInvocation.selector 方法名
// [anInvocation getArgument: NULL atIndex: 0] 参数
// 是否所有未查询到的方法 都会如此处理还是有什么要求呢？
- (void)forwardInvocation:(NSInvocation *)anInvocation {
   可以在这里面指定实现
   NSLog('test');
}

// 如何生成方法签名 可以利用对象生成方法签名，生成方法前面的规则 @"返回值类型@每一个参数从第几位开始:""

对于类方法的调用也是三个流程和实例方法时一致的但是由于，动态解析以及方法转发都是从消息接受者调用的此时由于
是类对象是消息接受者，因此方法都是类方法不同于实例对象的方法调用

@sythsize age = _age 并且自动生成set方法，现在会自动生成属性还有set和get方法
@dynamic 系统不会自动生成set和get方法也不会自动生成成员变量


**1.介绍一下OC的消息机制**
OC中的方法调用都是转换成了objc_msgSend函数的调用，给reciver（方法调用者）发送了一条消息（selector方法名）
objc_msgSend底层有三大阶段

消息发送（当前类，弗雷中查找） 动态方法解析、消息转发

**2.什么是runtime? 平时项目总有用过吗？**
NSProxy是一个专门用来进行消息转发的类。
[instance performaSelector: selector];
给NSObject 添加一个分类，在forwardIncovation：（NSInvocation *）anInvocation中处理所有unrecognized方法

**isKindOfClass isMemeberOfClassx相关面试题**
[anyClassObject isKindOfClass: [NSobject Class]] == 1;
这种情况下的只要是NSObject总是==1，因为NSObject metaClass superClass = [NSObject class];

左边实例对象  右边类兑现
左边类对象    右边元类














