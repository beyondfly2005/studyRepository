#JVM调优实战

> 视频地址：https://www.bilibili.com/video/BV1QV41167aX
> 马士兵教育

## P1 堆内存逻辑分区

熟悉GC常用算法，熟悉常见的来及收集器

## 1 GC和GCC Tuning
### 1.0.1 GC的基础知识
#### 1.0.0.1.1 什么是垃圾 
Garbage Collection
> C语言申请内存 mlloc free
> c++ new delete
> c/c++ 手动回收内存
> Java new ?
> 自动内存回收， 编程上简单，系统不容易出错，手动是否内存，容易出两种类型的问题
> 1 忘记回收
> 2 多次回收
> go lang 有垃圾回收器 ，并且STW
> Rust 不用回收垃圾 并且没有垃圾回收器
> 没有任何引用指向的一个对象或者多个对象(循环引用)

运行在JVM上的语言


#### 1.0.0.2.2 如何定位垃圾
1 引用计数 ReferenceCount
- 不能解决 循环引用问题 A引用B  B引用C  C引用A 一团垃圾
2 根可达算法 Root Searching
- 

什么是垃圾 what is garbag

怎么定位它

GC 





#### 程序 的栈（栈帧 stack frame）和堆

栈 先进后出

栈针 任何一个方法 都会有一个栈针

栈 （一个线程一个栈）

```
main{
	Object  o = new Object();
	m();
}
```

每个方法执行完毕 就会弹出栈针（将指针向下移动 向栈底部移动）



堆  

每一个线程都会维护一个线程栈

###### 栈和堆的区别：

栈的空间是自动释放的 ；方法结束，栈针向下移动，上面的栈空间自动释放

不需要手工释放。

堆 是执行过程中 手工创建出来的，不会自动释放，堆 空间随着对象的创建，有可能被占满

堆空间 需要手工回收，或者



#### 最难调试的问题：

- 野指针

  - 同一个对象两个指针，一个释放了，另一个不知道还拿来用，

  - 同一个指针，不同位置  

  - 不再执行任何对象的指针

  - NullPointerException

- 并发 问题

  - 多线程访问同一块内存空间

- 野指针+并发问题



#### 语言发展历史

- c / c++
  - 手工管理内存 malloc 创建内存空间 ，free释放；c++ 中的 new delete
  - 忘记释放 -memory leak 内存泄漏  内存没释放也不可用
  - 释放多次 产出产出及其难以调试的bug 一个线程空间莫名其妙被另外一个释放
  - 运行效率高，开发效率很多
- java python go js kotlin scala Lisp
  - 方便内存管理的语言
  - 引入了GC概念 Garbage Collector
  - 应用线程业务程序只负责分配不负责管理内存，GC线程负载回收内存
  - 大大的降低了程序员的门槛
  - java空指针问题 没有解决
  - GC回收会占用CPU时间，降低运行效率
- rust
  - 运行效率超高（对标asm汇编 c c++）
  - 不用手工管理内存 ，没有GC
  - 学习曲线巨高
  - ownership 所有权 任何一个值 归属于一个变量，有且只有一个
  - 多个变量指向同一个变量的事情不会发生
  - 只要程序语法通过，就不会有bug（语言层面的bug）

#### 聊聊JVM的GC的历史



#### 什么是garbage垃圾

没有任何 线程指向它，



怎么指向 

##### 一、引用计数法

循环引用 A指向B  B指向C  C指向A

###### 二、Root Searcheing 根可达算法 

什么是根  o = new Object() ; o就是根

树的搜索算法，找不到的就是垃圾，



##### 建立知识体系 囫囵吞枣



《JVMS JVM的 虚拟机规范》



三种算法：

##### Mark-Sweep 标记清除算法

从根开始找，找到的不是垃圾，找不到的是垃圾，标记出垃圾，记性

问题：内存碎片化严重

##### Copying 拷贝算法

内存只能占用一半，将一半复制到 另一半空间 

问题：浪费内存

##### Mark-compact 标记压缩算法





##### 三种算法都有自己的问题

垃圾回收器GC  综合使用三种算法

三种算法都有问题，三种的综合运用，诞生了各种各样的垃圾回收器



JDK1.0 -> JDK 10 一共10种

##### 1、Serial

把内存分为两个

##### 2、CMS

##### 3、ParNew

##### 4、Serial Old

##### 5、Parallel Scavenge

##### 6、



##### GC 的演化

GC的演化是随着内存的不断增长而演进



从JDK1.0到1.8 将内存分为两个区域

##### 新生代

​	 new  刚刚诞生的对象

##### 老年代

​		经历过一次垃圾回收的对象

新生代频繁扫描 ，老年代不用每次都扫，

针对不同对象的特点  使用

新生代 使用 复制算法

老年代 使用 

分带算法

​	一次年轻代的回收，会回收到90的对象

​	没必要使用copy算法 拷贝一半内存

将新生代 分为8:1:1

8=伊甸园  

1=survivor

1=survivor

survivor新生代

对象产生

第一次扫描 扫描伊甸园区+第一个suvr  活着的 放入 第二个suvr

第二次扫描 扫描伊甸园区+第一个suvr  活着活着的 

年轻代满了 就会

老年代满了就会产生FULL GC



永久代 是方 Clssz 放元数据用的



##### 分带算法

随着内存的不断增长而演进

几兆-几十兆

##### Serial算法

STW（stop the world） copying collector which users a singgle GC thread

工作在新生代 ，停止 使用单线程 业务进程停止 ， 进行垃圾回收

##### Serial old

老年代的算法

几十兆-上百兆

调优第一步换来及回收器

随着内存增大 

##### Parallel Scavenge 

​	工作在新生代

##### Parallel Old 

生产环境默认环境 Java1.8默认 是PS+PO (Parallel Scavenge + Parallel Old  )



如何知道使用哪种垃圾回收算法

```
java -XX：+ 


UseParalleGC
```



随着空间进一步增大

几G - 几十G

那么是不是增多垃圾回收的线程数 就可以了

并不是线程越多越好

16核 或者一核多线程 线程总是有数的

如果大大高于核心数 达到一定阈值时，会发生线程切换，和线程等待



##### Concurrent GC

CMS

ParNew

G1 替代CMS

ZGC

Shenandoah

这五个都是Concurrent GC



什么是Concurrent 

###### GC线程和业务线程可用同时运行

原来是GC线程一运行 业务线程 就要STW stop-the-world

停止。

Concurrent可能存在的 问题

标记不是垃圾的

没有标记已经变成垃圾的



##### CMS

##### 并发的标记清除

CMS工作在老年代

与之对应的工作在新生代的

ParNew 或者 Serial

不能是 



##### ParNew

a stop-zhe-word copying collector which uses multiple GC threads



##### CMS

四个阶段：

- 初始标记

  从根上开始找，只找根上的，依然是STW的 停止业务的 但是时间非常短，毕竟

- 并发标记

  错标：原理是垃圾 运行中有新新引用指向他

  ​			原来不是垃圾，但是随着释放 变成垃圾了

- 重新标记

  针对错标的 重新进行

- 并发清理

采用三色标记算法

GoLang采用三色标记算法





#### JVM调优实战

敲入一个java命令 就会启动一个 java虚拟机

\- 开头的 都是标准参数

-X开头的 都是非标参数 有的版本会有，有的版本没有，有的版本会有变化

-XX是不稳定参数  有的版本有 有的版本没有

```bash
java -XX:+PrintCommandLineFlags -version
```
Windows 输出
```
-XX:InitialHeapSize=367933504 -XX:MaxHeapSize=5886936064 -XX:+PrintCommandLineFlags -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:-UseLargePagesIndividualAllocation -XX:+UseParallelGC
java version "1.8.0_201"
Java(TM) SE Runtime Environment (build 1.8.0_201-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.201-b09, mixed mode)
```
Linux 输出

```
-XX:InitialHeapSize=46323264 -XX:MaxHeapSize=741172224 -XX:+PrintCommandLineFlags -XX:+UseCompressedClassPointers -XX:+UseCompressedOops 
java version "1.8.0_221"
Java(TM) SE Runtime Environment (build 1.8.0_221-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.221-b11, mixed mode)
```

InitialHeapSize 起始堆大小

MaxHeapSize 最大堆大小

UseCompressedClassPointers 使用压缩指针

UseCompressedOops 使用压缩普通对象指针

```
java -XX:+PrintFlagsInitial
java -XX:+PrintFlagsFinal -version
java -XX:PrintGC  打印垃圾回收信息
```

调优的步骤：

- 预调优

- 优化

- 问题的定位和处理

  

最大堆和最小堆调整为一致的，防止内存变化

年轻代YGC

老年代Full GC

频繁FULL GC 回收不了对象，

有可能OOM  （OutOfMemeryError）



###### 第一步：

top 命令查看系统中哪些进程一直在吃内存 占用cpu

###### 第二步：

jps命令 显示进程号

```
jps
```



```
jinfo 1879
```
线程栈的调用信息
```
jstack 1879
```

持有了一把锁 ，其他进程

```
top
jps 列出java进程
jsinfo  java进程号
jstack  定位思索
jstat -gc 1879  统计信息
jstat -gc 1879 500 每隔500毫秒
jmap 不能被工具替代的工具
```

java安装目录下 提供了两个图形化工具

jconsole.exe

jvisualvm.exe



内存抽象器：哪些对象占用最多

内部压力测试，通过远程压测

服务高可用，多台服务器，只监控一台服务器

TCPCopy 将显示服务器 copy到另一台非生产服务器，对非生产服务器进行监控

阿里开源的工具

```
arthas
```

启动

java -jar arthas-boot.jar

把arthas挂到进程上

help

```
# 第一个命令 dashboard
dashboard 替代top命令

#第二个命令jvm命令 
#替代jinfo jvm相关属性
jvm

#第三个命令 thread
thread
thread + 线程号

thread -b 查找线程死锁 活锁

heapdump 导出

jad 全类名 反编译  反编译会核对线上版本和自己的最新版本代码对比 是否版本被覆盖了

redefine /root/TT.class

```

哪些线程消耗内存和cpu最高

##### cpu 突然标高，怎么定位

arthas 业务线程

jvm线程 GC垃圾回收线程

Java开发手册 阿里 泰山版

线程池里面的线程起名字

ThreadFactory 工厂

jvm静悄悄的 退出，如何



JVM排查死锁

JVM



哪一个对象在吃内存



```
jmap  内存分析命令
jmap -histo 2058
jmap -histo 2058 | head -20 取前面20行
```

jmap 不能直接在线上用

会引起卡顿

jmap -dump:format=b,file=xxx.hprof 2366

将堆拷贝成文件 进行分析



hprof 文件如何分析

最简单是的使用jvisualvm.exe装入文件  jvisualvm 支持类sql 语句 okl来查询定位

jhat



