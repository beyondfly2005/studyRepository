## Java语言介绍

### Java发展简史

### Java的核心优势

​	java

​	跨平台是java语言的核心优势

​	

### Java 个版本的含义

JavaSE（Java Standard Edition）: 标准版，定位在个人计算机上的应用

​	这个版本是Java平台的核心，它提供了非常丰富的API来开发一般的个人计算机上的应用程序，包括用户界面接口AWT及Swing，网络功能与国际化，图像处理能力，以及输入输出支持等等

JavaEE（Java Enterprise Edition）：企业版 定位在服务器端的应用

​		JavaEE是JavaSE的扩展，增加了用于服务器开发的类库，如JDBC是让程序员能直接在Java内使用的SQL的语法来访问数据库内的数据，Servlet能够眼神服务器的功能，通过请求-响应的模式来处理客户端的请求；JSP是一种可以将Java程序代码内嵌在网页内的技术。

JavaME（Java Micro Edition）微型版，定位在消费电子产品的应用上

​		JavaME是 JavaSE的内伸

### Java的特性和优势

- 跨平台/可移植性
- 安全性
- 面向对象
- 简单性
- 高性能
- 分布式
- 多线程
- 健壮性

## JDK的安装和配置

### Java程序的运行机制

计算机高级语言类型主要有编译型的语言，和解释型的语言，而Java语言是两种类型的结合。

Java授信啊用文本编辑器编写Java源程序，源文件的后缀为.java;再利用编编译器javac



### JVM JRE JDK的区别

JVM （Java Virtual Machine）就是一个虚拟的用于执行bytecode字节码的虚拟

JRE（Java Runtime Environment） Java运行时环境包含Java虚拟机、类函数、库函数、运行Java应用程序锁必须的文件。

JDK（Java Develment Kit）Java开发工具包，包含JRE，以及

### Java开发环境搭建

#### JDK的下载和安装

#### 环境变量Path的配置

#### classpath配置问题

#### 测试JDK安装是否成功

开始 cmd 输入java -version

#### OpenJDK和JDK收费问题

2019年后，JDK8后续更新的版本开始收费了，但是主要针对企业用户

Oracle JDK

Open JDK

### 第一个程序

##### 开发第一个Java程序

##### 第一个程序常见的错误

​	javac不是内部或内部命令，也不是可运行的程序

​	

##### 第一个Java程序总结和提升

- Java对大小写敏感，如果出现了大小写拼写错误，程序无法运行
- 关键字public被称为访问修饰符 access modifier
- 

##### 最常用的DOS命令

​	cd

​	cd ..

​	dir

​	cls

​	Tab键

##### 常用Java开发工具

入门时可以使用文本编辑器

- Notepad++

- UltraEdit

- EditPlus

真正的学习开发中可以使用集成开发环境

	- Eclipse
	- IDEA
	- NetBeans

## 常用类

### 基本数据类型的包装类

自动装箱

Integer a=300;

//编译器Integer a= Integer.valueOf(300);

自动拆箱

int b=a;

//编译器c.intValue()

#### 1.4包装类的缓存问题

```java
Integer d1 = 400;

Integer d2=4000;

System.out.println(d1==d2); //两个不同的对象 引用比较

System.out.println(d1.equals(d2);// 值比较
                   
Integer d3 = 123;
Integer d3=123;
                   
//当数字在[-128,127] 之间的数字时 返回缓存数组之间的数据

//从缓存数组中取值。

```



