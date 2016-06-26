---
layout:     post
title:      "OC基础教程笔记"
subtitle:   ""
date:       2015-1-20
author:     "Bin"
header-img: "img/home-bg-o.jpg"
tags:
    - iOS
    
---

> “ OC基础教程笔记 ”


```objc
import保证文件只被包含一次。

bool是8位二进制，使用比较时用No只有0值，不要用yes。

oop间接操作，程序的数据为中心，函数为数据服务。

面向过程是建立函数，数据为函数服务。

类：是能实例化对象的结构体。对象通过它的类来获取各种信息，执行所需代码。

对象（实例）：是包含代码的struct，是包含值和指向其类的隐藏指针的结构体。

消息：对象可以执行的操作，通知对象做什么。

方法：为响应消息而运行代码。

id是泛型，可以引用任何类型的对象，是指向结构体的指针。

void * 返回对象可以是任何类型指针。

接口：是类为对象提供特性描述。

实例变量（成员变量）

[  ]方括号，通知对象做什么，发消息，前面是对象，后面是要执行的操作。

方法调度：（对象内有指向其类的指针）在当前类中找，（当前类内含有指向父类的引用指针）没有到父类找，都没有就到NSObject类中找，都没有就报错。

self：指向接受消息对象的指针，既类中第一个实例变量，指向isa指针的内存地址。

对象实例变量的内存布局，先是指向对象类的指针isa指针，父类实例，自身实例。

super：调用父类中的实现，不是参数和实例，既层层向上寻找。

NSlog：给对象发送description

alloc：分配内存，init：初始化对象。

get：获取对象属性

set：修改对象属性，
set get方法内不要用self，死循环。

继承：子类继承父类，获得父类所有功能，并可添加修改属性方法等，is a X是一个Y。

复合：通过包含实例变量的对象指针实现，对象的组合，has a X有一个Y。

oc对象交互都是通过指针。

.h文件之间引用会产生依赖关系，如果多个.h文件互相依赖，a.h引用b.h，b.h引用c.h，修改c，那么所有引用a的文件都得重新编译，所以.h的引用， 用@class前向引用，声明这是一个类，告知编译器我通过指针引用它，@class也可解决类的互相引用。

xcode：control＋i 重新排版
E枚举
f函数
#define
m方法
C类

com＋shift＋F

NSLog是暴力测试

OC对象是动态分配的，代价较大，所以数据类型是c结构体

+类方法，工厂方法
- 对象方法，实例方法

== 两对象是否为同一事物，对比指针数值
isequaltostring 是两对象否相等

foundation

NSString：
length
isequaltostring
compare：大小写、字符个数
hasPrefix
hasSuffix
rangeOfString

mutableString：对于可变，先设置capacity可提速
append
delete

NSArray：防止越界
count
objectAtIndex
componentsSeparatedByString字符串拆分数组。
componentsJoinedByString合并数组元素创建字符串。
可变：
add
remove

NSDictionary大数据快速查找
objectForKey

字典数组只能存对象，对基础数据类型封装NSNunber。

NSNumber 封装基本类型数据，@强转
+numberWithChar/Int/float/bool  转为NSNumber   装箱
- Char/Int/float/bool/stringValue 转为基本数据类型 开箱

NSValue 可以封装任意值，可以将结构体放入数组字典中，是 NSNumber 父类。
+valueWithBytes :(const void*)value objCType:(const char *)type;
第一个是&rect  后一个是@encode（NSRect）

point/size/rect Value

NSNull null 表示没有，nil不能用在数组字典。
枚举
1、NSEnumerator-objectEnumerator
-reverseObjectEnumerator
2、nextObject索引objectAtIndex
3、for in快速枚举
4、enumerateObjectUsingBlock代码块最快。

内存管理：
_weak关键字归零弱引用，指向对象清空后，自己归零。

strong
weak特性
assign
copy

alloc new copy 创建对象。
自动引用计数器。

属性名不能以new开头。
关键字和特性不能一起使用。

_bridge 传指针不传所有权
_bridge_retained 计数器加一
_bridge_transfer

alloc分配内存并将内存区域初始化0。
super init初始化超类
用懒加载

initWithContentOfFile
NSUTF8

判断语句nil写前面 nil ！= xx；

@property
@synthesize
@dynamic 不生成方法和变量

属性声明在.h是开放接口，声明在.m是私有接口，类扩展。

点语法 =左边set 方法， =右边get方法。

只读readonly的使用，只生成getter 不会生成setter。

分类 无法添加实例变量，分类方法名冲突，覆盖系统方法，给分类方法添加前缀。
利用分类根据不同功能进行分散逻辑，不同功能对应不同框架。

类扩展
分类可以访问其继承类的实例变量，方法优先级更高

block：
Int(^blockname)()=^(){};

可做变量：
typedef：

本地变量，block在定义时复制保存它们，不会修改。
__block ：修改block内的变量 但有限制，无长度可变数组，不包含可变数组的结构体。


```



