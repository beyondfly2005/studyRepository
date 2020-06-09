gargage

垃圾回收器



### 如何找到garbage

how to find a garbage

reference count 引用计数

没有任何引用的对象 

引用计数 无法解决 循环引用的问题

A->B->C  这三个除了互相引用之外 没被其他任何引用

Root Searching 根可达算法

GC roots 根包括哪些

线程栈变量

静态变量

常量池

JNI指针

JVM stack

native method stack

runtime constant pool

statice references in method area

Clazz



GC Algorithms 垃圾清除

- Mark-Sweep 标记清除

- Copy 拷贝

- Marj-Compact 标记压缩、



标记清除

​	优点 ：简单 缺点：碎片化









十种垃圾回收机制



CMS 

concurrent mark sweep  并行标记扫描

a mostly concurrent ,low-pause collector

4 ph

- init mark 初始标记STW
- concurrent mark 并发标记
- remark 重新标记STW
- 并发清理



老年代 碎片化

漏标和错标

三色标记算法 三色扫描算法

白灰黑

ZGC 方案

PSPO

jvm



分带算法

ParNew 年轻带  stop-the-world STW 是Parallel 算法的增强 为了配合CMS

CMS 老年代





内存少的时候 Serial 和 Serial Old配合

内存多带点的时候 Parallel Scavenge 和Parallel Old配合

内存再多的时候 ParNew 和 CMS配合

碎片话越来越多的时候 CMS会采用单线程

JDK8 默认算法Parallel

JDK9 默认算法G1

分布式锁  锁续命

锁续命 碰到垃圾回收 就完蛋了

分布式锁 使用的情况下  不建议使用PS和PO  推荐使用G1



ParNew = pS

专门配合 CMS使用 是对Par的优化变种



### JVM调优

- 根据jvm规划和预调优
- 优化运行JVM运行环境 （慢、卡顿）
- 解决JVM运行过程中的各种问题（OOM ）泄露 溢出



解决过OOM问题

​	java -Xms200M -Xms200M -XX+PrintGC com.mashibing.jvm.gc.T15_FiullGC-Prob

最大和最小设置成一样大

原因是 防止内存抖动，内存自动增大和缩小都会占用系统资源

linux top 命令 查看进程

内存越来越增长，垃圾回收也在回收 ，但是内存占用越来越来绝对

jconsole

jvisualvm

远程检测观察服务

压测环境 用的图形界面



arthas

 -Java 线上问题定位处理的终极利器

阿里开源的 

https://blog.csdn.net/u013735734/article/details/102930307



ls

cd arthas 3.1.4

java -jar artgas -boot.jar

Arrthas

dashboard



help 帮助



thread 查看线程

thread 39 调用堆栈 显示

thread help 

thread -b 查看死锁

JVM面试问题 线上运行的系统CPU突然飙高 怎么定位怎么解决

- 重启

- 列出线程 业务线程怎么处理 垃圾回收线程怎么处理

  业务线程 业务线程 

trace 跟踪线程

watch 



jmap命令

jps

jmap -histo 1196 将1196号进程对象信息打印出来

jmap -histo 1196 | head -20

列出占用内存最多的20个对象





线上系统 频繁的Full GC 怎么做



GU和ＧＣＴＵｎｉｎｇ

常见的垃圾回收器

如何定位问题

阿里Ａｒｔｈａｓ在线排查公交

Ｊｍａｐ工具　哪些对象占用最多

不同的垃圾回收器　参数怎么设置

多线程高并发

ＪＶＭ调优

ＭｙＳＱＬ优化

Ｈｏｓｔｏ

Ｈａｄｏｐ

Ｈｉｖｅ

Ｋｙｉｎ

ｎｅｔｔｙ

即时对战

和家服务云平台

亿级流量

网管

缓存　

１９年六月份

５－８个月知识点课程　　１８－２４个月　　８个项目

ＢＡＴ　

ＣＲＵＤ　－＞　

５月份会员免费升级

２０２０年升级　左程云　算法课

ＩＢＭ　百度　ＧｒｏｗｉｎＩＯ　ａｍａｚｏｎ

ＢＡＴＪ　ＴＭＤＰ牛客网内容

Ｐ８　算法数据结构　基础

堆结构

二分法

递归

贪心

动态规划

高配面试题训练营

第二个升级：　阿里美团　黄俊

落地淘宝

ＥＣＳ服务器　３０台服务器　１００个节点　服务器集群

模拟淘宝实际运行情况

前阿里　黄俊　＋　晁凤飞

分布式ＩＤ生成中心

基础类库

ＥＳ

ＭＱ

分布式调度

ＭＹＳＱＬ集群



应届生：　算法（每天必须缓慢增长　ｌｅｅｔｃｏｄｅ）

－　设计模式

－　多线程

－　ＪＶＭ

ＲＥＤＩＳ

ＺＫ

ＭＹＳＱＬ调优

－－－－－－－－－－－－－大厂面试－－－－－－－－　

Ｎｅｔｔｙ网络　网游后端

分布式微服务

简历指导



网约车项目

亿级流量





持续性成长　Ｐ８课程来学



掌握Ｊａｖａ核心　有良好的算法和编码鞥努力

精通编码对象编程





项目

架构

高并发

数据结构与算法

ＧＣ　ＪＶＭ



安卓　ｐｈｐ　ｐｙｔｈｏｎ　ｇｏ　ｃ＋＋　ｉｏｓ　前端

天花板

ｇｏ和　ｃ＋＋天花板比较高

架构师　百万以上

ｊａｖａ天生有架构师基因

网络接入层　

ＡＰＩ网关层



数据访问层

缓存层

数据存储层



越早加入架构知识　天花板网上



ＣＭＳ和Ｇ１的异同

Ｇ１什么事时候应ＦｕｌｌＧＣ

说一个垃圾回收算法

吞吐量调优

ＴｈｒｅａｄＬｏｃａｌ有没有内存泄露的问题　　弱指针存在　才会解决内存泄露

ＣＭＳ并发预处理和并发可中断预处理

多大的对象会被扔到老年代　

用一句话说明ＪＶＭ水平

​	十种ＧＣ算法全部精通

数据ＧＣ的常用算法　熟悉常用的垃圾收集器　具有世界ＪＶＭ调优实战经验



