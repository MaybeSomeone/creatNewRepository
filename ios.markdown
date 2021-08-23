# iOS Test

// https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ObjectiveC/Chapters/ocObjectsClasses.html#//apple_ref/doc/uid/TP30001163-CH11-SW1

# NSOBject底层原理及实现
> NSObject的概念：NSObject本质是一个结构体,他作为对象的蓝图，定义了所有类的基本接口和特诊。
> Obj-c是代码本质C\C++代码，他的面相对象是基于C\C++数据结构中的结构体实现的。

查看源码的方式： xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main-arm64.cpp

arm64 支持iPhone5s及以上  
armV7 支持iPhone4及以上  
armv7s 支持iPhone5及以上  
i386 模拟器

x86则是基于因特尔的处理器的台式机器 mac

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
*1.instance的isa指向class*  
当通过调用对象方法时通过isa找到class,最后找到对象方法进行调用  
 
*2.class 的isa指向meta-class*  
当地调用类方法时，通过isa找到meta-class找到类方法进行调用  

class的superClass有什么用？  
NSObject-> metaClass-> superClass = 指向类对象  

*3.metaClass中的isa全部指向基类的meta-class，根meta-class isa指向自己，superClass指向类对象。*  

在调用函数，发送消息时，实际上经过编译之后是不会区分实例方法和类方法的，只会根据方法名进行识别。  

metaClass中的isa有什么用途？  


## isa中储存地址是否指向的就是类对象的地址?  
不是！！现在版本实例对象和class以及metaClass中的isa全部都需要再&ISA_MASK才能得到class或者metaClass真正的地址值。注意superClass则储存的是正确的地址值。  

# oc的类信息存放在哪里？  
1.对象方法 属性 成员变量 协议信息 存放在class对象中  
2.类方法 存放在meta-class对象中  
3.成员变量的具体值，存放在instance对象中  
 
```
// c++结构体和类几乎没有区别
struct objc_object {
  isa_t isa // === Class ISA
}


struct objc_class: objc_object {
  // Class ISA;
  Class superClass;
  cache_t cache; // 方法缓存
  class_data_bits bits; // 用于获取具体的类信息 &FAST_DATA_MASK
  // 存储方法信息 成员变量信息
  class_rw_t *data() {
    return bits.data(); // === bits & FAST_DATA_MASK === class_rw_t
  }
}

strcut class_rw_t {
  const class_ro_t *ro;
  method_t
}

struct class_ro_t {
  
}
```


// 窥探内存的方案，通过定义一套相同结构的定义在debug下观察数据的结构和地址
