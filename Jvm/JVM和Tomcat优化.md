# JVM和Tomcat优化

> 视频地址：

```properties
01——GCC		https://www.bilibili.com/video/BV1Gb411C7h8
02--GCC 	https://www.bilibili.com/video/BV1Vb411C7j4
03--Tomcat 	https://www.bilibili.com/video/BV1Cb41167c4
04--Tomcat	https://www.bilibili.com/video/BV1kb41167hR
05--Tocat	https://www.bilibili.com/video/BV1Cb41167Az
06--Tocat	https://www.bilibili.com/video/BV1Ub411z7xN
07--Tocat	https://www.bilibili.com/video/BV1Ub411z7pN
08--压测	   https://www.bilibili.com/video/BV1Ub411z7HE
09-APR		https://www.bilibili.com/video/BV1Ub411z7xN
```

​	

### JVM的配置和优化

- **GCC是什么？ **

  垃圾回收

- **分几种？分别是什么？**

  JVM在进行GC时，并非每次对上面的三个内存区域（新生代、老年代、永久代）一起回收的，大部分时候回收的都只新生代。

  因此GC安装回收的区域又分为两种类型，一种是普通GC(minor GC)，一种是全局GC(major GC)

  - 普通GC（minor GC）：只针对新生代区域的GC
  - 全局GC(major GC or Full GC)：针对老年代的GC，偶尔伴随着对新生代的GC以及对永久代的GC.

- **分别在什么地方**

  - 频繁手机Young区

  - 较少收集Old区

  - 基本不动Perm区

    

GC回收的三大算法

- 引用算法

  已经不用

- **新生区的复制算法**：MinorGC（普通GC）

  年轻代中使用的是Minor GC ，这种GC算法采用的是复制算法（Copying）

  原理：

-XX:MaxTenuringThreshold  - 设置对象在新生代中存活的的次数 默认15

1个Eden区2个Survivor区比例为：8:1:1

优点：

​	不产生碎片

缺点：

​	浪费他浪费了一半内存，这太要命了。1/10内存

​	如果对象的存活率很高

**谁空谁是To；From To 交换**

To区被填满，填满后放到老年代中

- **老年代的标记清除算法（Mark-Sweep）**

  标记：从根集合进行扫描，对存活对象形进行标记

  清除：扫描整个内存空间，回收未标记的对象，使用free-list记录可以区域

  优点：不需要额外空间

  缺点：两次扫描，耗时严重

  ​			会产生内存碎片

- **标记清除-整理算法（Mark-Compact）**

  缺点：效率不高
  
  
  
  并发清除标记算法CMS 是标记清楚算法的多线程实现

新生区用复制

养老钱有标记-整理算法

没有最好分代收集算法

### Tomcat的配置和优化

#### Tomcat之JVM内存查看

- Tomcat6的user配置

- Tomcat7的user配置

  ```bash
  #启动tomcat 
  ./startup.sh
  
  #tomcat 图形化版本控制器
  http://localhost:8080/manager/status
  用户名：root 密码 不知道
  
  401 Unauthorized 未被授权
  conf/tocat-user.xml
  
  # 停止tomcat
  ./shutdown.sh
  
  # 查看端tomcat 是否启动 方法1
  ps -ef |greop 8080
  ps -ef |greop tomcat
  # 查看端tomcat 是否启动 方法2
  netstat -nlp |grep 8080
  netstat -ntulp |grep 8080
  # 查看端tomcat 是否启动 方法3
  lsof -i8080
  
  cd conf
  # linux安装路径不允许有中文 不能有空格
  vim tomcat-users.xml
  #show linenum 显示行号
  :set nu
  
  # 添加如下内容
  <role rolename="admin-gui" />
  <role rolename="manager-gui"/>
  <user username="admin" password="admin" roles="admin-gui,manager-gui“/>
  
  #保存退出
  :wq 
  ```

  JVM内存物理内存的四分之一

  Eden Space

  Old Gen

  Survivor Space

  Code Cache

  Perm Gen

  

  ajp-bio-8009 阻塞IO

  

  并发数200

  

  **优化Tomcat**

  ** 备份**

  cp catalina.sh catalina.sh.bak

  修改catalina.sh

  ```
  
  ```

  Nginx

  - 负载均衡

  - 动静分离

  - 反向代理

    > 正向代理

    以请求端而言 ，我自己去找服务、我自己知道我用了代理：蓝灯 佛跳墙 红杏出墙 云梯 等翻墙软件访问谷歌网站，叫

    > 反向代理

    以请求端 我不知道百度 淘宝 使用哪台服务器给我提供的服务，

    我拨打10086 电话，我不知道哪个话务员给我提供服务，

  

  **WebService**

  ​	落地的实现 常用的有四种

  - **JDK**

  - **CXF**

  - **Axis1**

  - **Axis2**

  - **xfire**

  WebServer

  ​	Nginx / Apache

  AppServer

  ​	Tomcat/WebLogic/IBM WAS

#### Tomcat之启动优化

  ```bash
  # 查看tomcat进程号
  ps ef|grep tomcat
  
  # java堆内存命令
  jmap -heap 15622
  
  # java javac jmap
  
  Heap Configuration
  	MinHeapFreeRatio = 40 	堆内存最小空闲百分比
  	MaxHeapFreeRatio =70 	堆内存最大空闲百分比
  	MaxHeapSize
  	NewSIze= 新生区			Yong代内存默认容量
  	OldSize=
  	NewRatio=2				Yang Old区内存比例    
  	SurvivorRatio=8			Yong代中的Eden/Survivor的比例
  	PerSize=128M
  	MaxPermSize=256M
  	G1HeapRegionSize=0
  	
  New Generation
  
  Eden Space:
  From Space
  To Space:

  ```

  参数逐项说明

  ```
  exprot JAVA_OPTS=""  一行
  
  export JAVA_OPTS="-server -Xms1600M -Xss512k -XX:+AggressiveOpts -XX:+UseBiasedLocking -XX:PermSize=128M -XX:MaxPermSize=256M -XX:+DisableExplicitGC -XX:MaxTenuringThreshold=31 -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:+CMSParallelRemarkEnabled -XX:+UseCMSCompactAtFullCollection -XX:LargePageSizeInBytes=128M -XX:+UseFastAccessorMethods -XX:UseCMSInitiationOccupancyOnly -Djava.awt.headless=true"
  
  -server 启用jdk的server版
  -Xms-Xmx  永远一致 不设置区间值 避免内存的忽高忽低 设置为一致是
  避免每次GC后调整堆的大小，
  -Xss 设定每个线程栈的大小，一般设置不超过1M，要不然就会出现out of memory
  -XX:+AggressiveOpts	JDK优化的魔法属性 可以将最新版的JDK优化后的新特性自动注入
  -XX:+UseBiasedLocking	启用一个优化了的线程锁，对于高并发访问很重要，太多的请求忙不过来它自动优化，对于各自长短不一的请求，出现阻塞、排队现象，他自己优化。
  -XX:PermSize -XX:MaxPermSize
  	永久代内存 可以调整256 512
  -XX:MaxNewSize 最大的新生代大小 默认是16M
  -XX:NewSize 设置年轻代内存大小 建议交给JDK来设置
  -XX:+DisableExplicitGC
  	在程序代码中不允许有显示的调用System.gc() 避免内存的大起大落
  	忽略手动调用GC的代码使得Sysem.gc()的调用会变成一个空调用，完全不会触发任何GC
  -XX:MaxTenuringThreshold=31
  	默认15 生成
  -Djava.awt.headless=true	
  	用于放到最后一个 并且永远是true
  	这个参数我们一般放到最后使用，这个参数的作用是这样的，有时我们会在j2ee工程中，使用一些图标工具jfreechart 用于在web网页输出gif/jpg等流
  	在windows环境下，一般我们的app server在输出图形是不会碰到什么问题
  	但是在linux unix环境下会碰到一个exception导致你在windows开发环境下的图片显示的好好的，可是在linux unix下却显示不出来，因此加上这个参数可以避免这样的情况出现
  -XX:+PrintGCDetails 
  	打印GC收集过程
  	
  ```

#### Tomcat之并发优化

##### 位置

```bash
cd /tomcat7/conf/server.xml
cp server.xml server.xml.bak
```



##### 优化

```bash

# 默认协议
protocol="HTTP/1.1"
改为 (标准版 足以应付一般的并发请求)
protocol="org.apache.coyote.http11.Http11NioProtocol"
maxThreads="600" 
minSpareThreads="100"	最小备用线程数
maxSpareThreads="500"	最大备用线程数
acceptCount="700"
```

- 复杂版

```xml
<Connector port="8080"
    proctocol="org.apache.coyote.http11.Http11NioProtocol"
    URIEncoding="UTF-8"
    minSpareThreads="25"
    maxSpareThreads="75"
    enableLookups="false"
    disableUploadTimeout="true"
    connectionTimeout="20000"
    acceptCount="300"
    maxThreads="300"
    maxProcessors="1000"
    minProcessors="5"
    useURIValidationHack="false"
    compression="on"
    commpressionSize="2048"
    compressableMinType="text/html,text/xml,text/JavaScript,Text/css,text/plain"
    redirectPort="8443"/>
```

##### 参数逐项说明

```
minSpareThreads="100"	最小备用线程数
	Tomcat初始化时创建的线程数，最小备用线程数
	
maxSpareThreads="500"	最大备用线程数
	一旦创建的线程数超过这个值，Tomcat就会关闭不再需要的socket线程

enableLookups="false"	是否允许使用DNS查询，通常情况下设置为false
	如果希望调用request.getRemoteHost()进行DNS查询，以返回远程客户机的时间主机名，将enableLookups设置为true
	如果希望忽略DNS查询 仅仅返回IP地址 设置为false 这样设置提高了性能
	缺省情况下，DNS查询是key使用的
	一句话：是否反查域名，取值为：true或false 为了提高处理能力，一般设置为false

disableUploadTimeout="true"	
	
connectionTimeout:网络连接超时

acceptCount 是当线程数达到maxThread后，后续请求会被放入一个等待队列，这个acceptCount是这个队列  就直接refuse connection

maxThreads 最大线程数 默认200 配置600  保守推荐600-900

minProcessors
maxProcessors 最小线程数 最小的处理线程数，及时没有任何http请求，Tomcat也至少保持至少这么多的线程等待处理
	Accept Count
减少一些url的不必要的url检查从而减小开销

compression="on" 压缩 是否开启GZip压缩 建议开启 设置为 on
compressionMinSize="2048"	压缩 2kb
compressionableMimeType="text/html,text/xml,text/JavaScript,text/css,text/plain" 压缩类型 


```

- 坑爹情况

只装1个JDK，至少每个盘只装1个JDK 不要多个放同一个盘

catalina.sh脚本

指定Java_home

set JAVA_OPTS=

set JAVA_HOME=D:\Program Files\Java\jdk1.7-64\jdk1,7,0_67_x64



- linux下修改catalina.sh

export  JAVA_OPTS=xxxx

- windows 下修改catalina.bat

export 改为set

set JAVA_OPTS=xxxx

##### 超时控制

修改conf/web.xml配置文件设置session-timeout的值（单位：分钟）

- session-timeout 默认30分钟
```xml
<session-config>
    <session-timeout>30</sesssion>
</session-config>    
```
#### Tomcat之内存优化

##### 查看日志是否有内存溢出

查看%TOMCAT_HOME%\logs文件夹下，日志文件是否有内存溢出错

##### Java heap space

错误提示：java.lang,OutOfMemortError: Java heap space

- 导致原因
- Tomcat默认可以使用的内存为128M，在较大型的应用项目中，这点内存是不够的，有可能导致系统无法运行，常见的问题是包Tomcat内存溢出错误，Out Of Memory(系统内存不足)的异常，从而导致客户端显示500错误。

  异步调整Tomcat的使用内存即可解决此问题。

  public static void main(String[] args){

  ​	System.out.println(Runtime.getRuntime().maxMemory()/1024/1024+"M")

  ​	byte[] byteArreay = new byte[1*1024*1024*650];	//创建了一个大对象 测试是否溢出

     System.out.println("#######3");

  }
  
- windows环境下的修改
```properties
# 修改%TOMCAT_HOME%\bin\catalina.bat 文件，在文件开头增加如下设置
set JAVA_OPTS=-Dfile.encoding=UTF-8 -sever -Xms1024m -Xmx=2048m -XX:NewSize=512M -XX:MaxNewSize=1024m -XX:PermSize=256m -XX:MaxPermSize=256m -XXP:MaxTenuringThreshold=10 -XX:NewRatio=2 -XX:+DisableExplicitGC

```
- linux环境下的修改
测试jvm最大能配置多大内存

```bash
java -version
java -Xmx2048m -version  
java -Xmx3096m -version
```

配置内存

```properties
# 修改%TOMCAT_HOME%/bin/catalina.sh 文件
exprot JAVA_OPTS=-Xms2048m -Xmx2048m
```

##### PermGen space

错误提示：java.lang.OutOfMemoryEoor:Permgen space

- 导致原因

  Permgen space 全称是Permanent Generation space，是指内存的永久保留区域，这块内存主要是被JVM存放Class和Meta信息的，Class再被Loader时，就会被放到PermGen Space中，它和存放类实例（Instance）的Heap区域不同，GC（Garbage Collection）不会再主程序运行期对PermGen Space进行清理，索引如果你的应用中有很多CLASS的话，就很可能出现PermGen space错误，这种错误常见在web服务队JSP进行pre-commpile的时候，如果你的WEB APP下引用了大量的第三方jar，其大小超过了jvm默认的大小（4M）那么就会产生此错误信息。

  

- windows环境下的修改

```properties
# 修改%TOMCAT_HOME%\bin\catalina.bat 文件，在文件开头增加如下设置
set JAVA_OPTS=-Xms256m -xmx256m -XX:MaxNewSize=256M -XX:PermSize=128M -XX:MaxPermSize=256m
```



- linux环境下的修改

```bash
# 修改%TOMCAT_HOME%/bin/catalina.sh 文件
exprot JAVA_OPTS=-Xms256m -Xmx256m -XX:MaxNewSize=256m -XX:PermSize=128m -XX:MaxPermSize=256m
```



#### apache的ab压力测试

###### Apache服务器安装

```bash
# 下载地址： http://archive.apache.org/dist/httpd/
wget http://archive.apache.org/dist/httpd/httpd-2.2.29.tar.gz

# /opt下解压 httpd-2.2.29.tar.gz
cd /opt
tar -zxvf httpd-2.2.29.tar.gz

mkdir -p /user/local/ewb/apache

cd /opt/httpd-2.2.29

gcc -v

./configure --prefix=/usr/local/web/apache --enable-shared=max --enable-module=rewirte --enable-module=so

make 
make install

cd /usr/local/web/apache/bin
ls

ab -n1000 -c100 http://localhost:8080/
	-n=number 1000人 
	-c 并发 每100人一组 分10组 每次一组100人
```



### 网络传输优化



### 