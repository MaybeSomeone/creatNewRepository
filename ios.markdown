# iOS Test

// https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ObjectiveC/Chapters/ocObjectsClasses.html#//apple_ref/doc/uid/TP30001163-CH11-SW1

# NSOBject底层原理及实现
> NSObject的概念：NSObject本质是一个结构体,他作为对象的蓝图，定义了所有类的基本接口和特诊。
> Obj-c是代码本质C\C++代码，他的面相对象是基于C\C++数据结构中的结构体实现的。

查看源码的方式： xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main-arm64.cpp

### 1.一个NSObject对象占用多少内存？
64bit系统
pointer 占8个字节
int 四个字节
doule\float\NSString 八个字节

正确结果实为16个字节

NSObject 成员变量为8个字节，实际利用的也是8个字节，但是占用为16个字节，core-foundation强制至少分配16个字节（64Bit环境下）。

// 计算结构体大小内存对齐
**内存对齐： 结构体的内存大小是最大内存占用成员的整数倍**

// 操作系统计算内存对齐
**内存对齐：Buckets桶（16,32,28,64,80,96最大256）iOS中给OC对象分配内存大小都是16的倍数**

**classGetInstance()**：这个方法返回一个对象至少需要多少内存
**malloc_size()**：返回系统实际分配内存大小

***结论：我们在计算某个对象占用内存大小时，应先计算其结构体大小，然后根据系统分配内存原则计算它实际分配内存的大小。***

```
// 实例对象的结构 NSObject implementation
@interface NSObject {
  Class isa
}

// 继承结构中结构体实际变量
struct Person_IMPL {
  struct NSObject_IMPL NSObject_IVARS;
  int _age;
}

struct Student_IMPL {
  struct Person_IMPL Person_IVARS;
  int _no;
}

strcut NSObject_IMPL {
  Class isa;
}

```

// iOS为小端模式，从高地址开始读取进行排列，也就是说会从后面的地址开始读入数据，大端模式则相反

//实例对象仅存储成员变量没有存储方法信息

classGetInstanceSize -> alignInstanceSize

// gnu is not unix
sizeOf(argument)它是一个运算符，在编译期就可以获得该变量占用空间大小。 

### 类对象
每个类的类对象有且只有一份
每个类的元类对象有且只有一份

类对象存储的信息？
isa指针
superClass指针
类的协议信息  类的对象方法信息
类的协议信息  类的成员变量信息

对象的分类
instance（实例对象）
class （类对象）
meta-class （元类对象）

如何获取元类对象？
// 从类对象中获取meta class，该方法总是会返回类的类对象
Class objectMetaClass = object_getClass(objectClass)

// class 方法返回的一直是class对象，不管调用多少次都是class对象
[[objectClass class] class];

// class和metaClass内存结构一致，仅用途不同

metaClass中所存储的信息
isa
superClass
类方法信息
...

classIsMetaClass();


# 对象的isa指向哪里？
instance的isa指向class
当通过调用对象方法时通过isa找到class,最后找到对象方法进行调用

class 的isa指向meta-class
当地调用类方法时，通过isa找到meta-class找到类方法进行调用

class的superClass有什么用？


# oc的类信息存放在哪里？





