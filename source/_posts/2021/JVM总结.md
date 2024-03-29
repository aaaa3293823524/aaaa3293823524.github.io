---
title: JVM总结
tags:
  - 搭建博客
  - 前端
date: 2021-11-05 20:50:12
abbrlink: 80p6q
---
debug              vmoptions                     
step into     f7

java二进制字节码运行环境    native调用操作系统完成任务

一次编写，到处运行
自动内存管理，垃圾回收

理解底层原理    定位分析  解决

字节码结构   jvm指令

解释器(翻译成机器码)   JIT即时编译器

每个线程只能有一个活动栈帧   对应正在执行方法  
栈：线程运行需要内存空间
栈帧：每个方法运行时需要的内存    （参数  局部变量  返回地址）   局部变量线程安全   看作用域      基本数据类型？

内存溢出 （栈帧过多  栈帧过大）



线程诊断
1 cpu占用高        
定位
 top     ps H -eo  pid,tid,%cpu|grep  
jstack

2 迟迟得不到结果     线程死锁

本地方法栈     调用c,c++方法的空间   像wait  clone等

堆   -Xmx   -Xms
堆内存溢出 

jps
jmap    某一时刻    jmap -heap    内存快照信息
jconsole    实时监测  图形界面  多功能   直观

垃圾回收后内存占用依然很高
jvisualvm   堆转储（内存快照）  分析大对象

方法区（概念）   1.6  用堆内存永久代作为方法区    1.8用操作系统内存  元空间    类  类加载器  常量池
以前导致永久代内存溢出
之后元空间内存溢出

直接内存

String Table


分析GC日志


简历熟悉

一个.java文件在编译后会形成相应的一个或多个Class文件（若一个类中含有内部类，则编译后会产生多个Class文件），但这些Class文件中描述的各种信息，最终都需要加载到虚拟机中之后才能被运行和使用。事实上，虚拟机把描述类的数据从Class文件加载到内存，并对数据进行校验，转换解析和初始化，最终形成可以被虚拟机直接使用的Java类型的过程就是虚拟机的 类加载机制

类加载时机？步骤

垃圾收集器工作流程   场景
class类文件结构   字节码指令执行引擎
先行发生原则   happen before

as if serial
不管怎么重排序（编译器和处理器为了提高并行度），（单线程）程序的执行结果不能被改变

CMS:是一种以获得最短回收停顿时间为目标的收集器，标记清除算法，运作过程：初始标记，并发标记，重新标记，并发清除，收集结束会产生大量空间碎片；

三色标记法  https://blog.csdn.net/CSDN_WYL2016/article/details/108440494


happens before 
看书：
java并发编程的艺术
java多线程编程核心技术
图解HTTP
图解TCP/Ip
spring技术内幕
深入浅出MyBatis技术原理与实战
redis设计与实现

牛客网刷动态规划

gc日志
cpu占用
jstat可以打印出当前JVM运行的各种状态信息

jstack可以生成当前JVM的线程快照，也就是当前每个线程当前的状态及正在执行的方法，锁相关的信息
jstack -l 进程id

jmap
这个命令可以生成当前堆栈快照。使用 jmap -heap 进程id可以打印出当前堆各分区内存使用情况的情况，新生代(Eden区,To Survivor区,From Survivor区)，老年代区的内存使用情况。

jmap -histo 进程id 打印出当前堆中的对象统计信息，包括类名，每个类的实例数量，总占用内存大小。

执行jvisualvm命令打开使用Java自带的工具Java VisualVM来打开堆栈快照文件，进行分析。可以用于排查内存溢出，内存泄露问题 在Java VisualVM里面可以看到每个类的实例对象占用的内存大小，以及持有这个对象的实例所在的类等等信息。

也可以配置启动时的JVM参数，让发送内存溢出时，自动生成堆栈快照文件。

使用jmap -dump:format=b,file=/存放路径/heapdump.hprof 进程id就可以得到堆转储文件，然后执行jvisualvm命令就可以打开JDK自带的jvisualvm软件

MAT主要可以用于分析内存泄露，可以查询dump堆转储文件中的对象列表，以及潜在的内存泄露的对象。hprof文件，主页会展示潜在的内存泄露问题



卡表 rememberset  跨代引用
https://www.cnblogs.com/hongdada/p/14578950.html

类加载过程
https://blog.csdn.net/u010942465/article/details/81709246

文件格式验证（符合class文件格式规范）、元数据验证（语法）、字节码验证（语义）和符号引用验证


准备阶段是证实为类变量分配内存并且设置初始化值的阶段
这个阶段进行初始化的数据只有静态字段，并且是赋值初始化值(final修饰的字段除外)


初始化阶段是类加载过程的最后一步，这个阶段才开始真正的执行用户定义的Java程序。在准备阶段，变量已经赋过一次系统要求的初始值，而在初始化阶段，则需要为类变量(非final修饰的类变量)和其他变量赋值，其实就是执行类的<clinit>()方法

在Java语言体系中，<clinit>()不是一个合法变量，这个方法也不是由用户显示定义的，是由编译器生成的，编译器在编译阶段会自动收集类中的所有类变量的赋值动作和静态语句块(static{})中的语句合并而成的，编译器收集的顺序是由语句的顺序决定的，静态语句块只能访问到定义在静态语句块之前的变量，定义在静态语句块之后的变量，可以赋值，但是不能访问。

<clinit>()方法对于类和接口来说不是必须的，如果类或接口中没有定义类变量，也没有静态语句块，那么编译器将不为这个类或者接口生成<clinit>()方法，如果类或者接口中生成了<clinit>()方法，那么这个方法在执行过程中，虚拟机会保证在多线程环境下的线程安全问题。


操作码 操作数

增量更新：黑色对象新增一条指向白色对象的引用，那么要进行深入扫描白色对象及它的引用对象。

原始快照：灰色对象删除了一条指向白色对象的引用，实际上就产生了浮动垃圾，好处是不需要像 CMS 那样 remark，再走一遍 root trace 这种相当耗时的流程。

SATB可能造成更多的浮动垃圾


GC调优

select和poll


继承树，底层数据结构，线程安全，执行效率

ArrayList  并发修改 ConcurrentModificationException异常   默认会是10

SynchronizedList   CopyOnWriteArrayList


如果不指定初始容量，HashMap和ConcurrentHashMap默认会是16，HashTable的容量默认会是11    

LinkedHashMap

expectedModCount

一致性hash

 所以有了一致性hash，一致性hash就是创建出n个虚拟节点，n个虚拟节点构成一个环，从n个虚拟节点中，挑选出一些节点当成真实的upstream server节点。构成一个每次将计算得到的hash%n，得到请求分配的虚拟节点的位置c，从位置c顺时针移动，获得离c最近的真实upstream server节点。

这样请求分配时就会比较均匀，而且upstream server的数量变化只会影响计算出key值hash与其”最近”的预分配的虚拟节点。
