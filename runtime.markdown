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






















