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





























