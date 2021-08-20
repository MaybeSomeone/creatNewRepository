# iOS Test

// https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ObjectiveC/Chapters/ocObjectsClasses.html#//apple_ref/doc/uid/TP30001163-CH11-SW1

# NSOBject底层原理及实现
> NSObject的概念：NSObject本质是一个结构体,他作为对象的蓝图，定义了所有类的基本接口和特诊。
> Obj-c是代码本质C\C++代码，他的面相对象是基于C\C++数据结构中的结构体实现的。

查看源码的方式： xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main-arm64.cpp

### 1.一个NSObject对象占用多少内存？
64bit系统
pointer 占8个字节
正确结果实为16个字节

NSObject 成员变量为8个字节，实际利用的也是8个字节，但是占用为16个字节，core-foundation强制至少分配16个字节（64Bit环境下）。


```
// 实例对象的结构 NSObject implementation
@interface NSObject {
  Class isa
}


strcut NSObject_IMPL {
  Class isa;
}

typedef struct objc_class *Class 



```











