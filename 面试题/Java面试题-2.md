> https://www.bilibili.com/video/BV1k74118721?from=search&seid=12702565931832666220

01 谈谈对面向对象思想的理解

02 JDK JRE JVM

03 ==和equeals的区别

==比较的是值，

比较基本数据类型，比较的是数值，equals比较的是内存地址

equals默认比较的是地址，因为这个方法的最初定义在Object方法上

##### 04 final秀是覅类 不可变 不可继承 不可扩展

比如String String 不可改变

```java
final Student student = new Student(1,"Andy");
student.setAge(10); //可以

```

##### 6 String StringBuffer StringBuilder

StringBuffer是线程安全的

什么是线程安全

1、多线程环境下

2、对这个对象方法不需要加入额外的同步控制

3 、操作的数据的结果依然是正确的

StringBuffer的每个方法的源码 都加入了synchronized修饰符

优先StringBuilder 》 StringBuffer 》String

什么情况下 线程不安全：

单线程情况下 不需要考虑 可以使用Stringbuilder

每次调用方法 创建一个栈针，

通常情况下 在service下的方法里使用StringBuilder

每个线程访问一个StringBuilder

这种情况下 是线程安全的 ，一个线程访问一个资源

HashMap ArrayList StringBuilder 都是线程不安全的 但是在Service中的方法中大量使用 它是没有问题的 不用考虑线程安全。

#### 7 接口和抽象类的区别

语法区别 

方法可以有抽象的 也可以有非抽象的 有构造器

JDK1.8之后 

接口里面有可以实现的方法，在方法上加锁default和static

带来的好处是 减少了很多Adapter层的东西

设计上考虑

抽象类  同一事务的抽取

UserDao DeptDao 抽取BaseDao 可以写具体的实现

JDK 

接口是一种契约，

从单体架构 分层开发