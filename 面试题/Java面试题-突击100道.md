# Java面试题 突击100道

>  https://www.bilibili.com/video/BV1k74118721

##### 01 谈谈对面向对象思想的理解

```textmate
面向对象 是组织这的思维模式
面向过程 是执行者的思维模式

面向对象的思维 在项目开发中，三层架构的模式 为了解决问题 而不是

三大特性：封装、继承、多态 

```

##### 02 JDK JRE JVM

```text
JDK 是Java开发开发包 Java Development Kit 提供Java的开发环境和运行时环境 还包括编译Java源文件的Javac 还有调试和分析工具
JRE 是运行时环境 Java Runtime Environment 包含虚拟机和一些基础的类库 
JVM 是虚拟机 提供执行字节码文件的能力，Java Virtual Mashine
JDK 包括 JRE和JVM
JVM是实现跨平台的核心，但是JVM本身并不是跨平台的，不同平台需要安装不同的JVM
```

##### 03 ==和equeals的区别

```text
==比较的是值，
比较基本数据类型时：比较的是数值，
比较引用类型：比较引用指向的地址

equals比较的是内存地址

equals默认比较的是地址，因为这个方法的最初定义在Object方法上，默认的实现就是比较地址
自定义的类，如果需要比较的是内容，就需要像String，重写equals方法
```

##### 04 final修饰类 表示不可变 不可继承
```
final修饰类时，表示不可变，不可继承，不可扩展
比如String  不可改变

final修饰方法时，表示该方法不可重写
比如模板方法，可以固定我们的算法

final修饰变量，这个变量就是常量
注意：
- 修饰的是基本数据类型，这个值本身不能修改；
- 修饰的是引用类型，引用的指向不能修改

比如下面的代码是可以的
```

```java
final Student student = new Student(1,"Andy");
student.setAge(10); // 可以 没有改变指针的指向 只改变了 
student = new Student(); //不可用 改变了指向

```

##### 5 String StringBuffer StringBuilder

String 与另外两个类的区别是
> String 是final类型，每次声明的都是不可变的对象，
> 索引每次操作都会产生新的String对象，然后将指针指向新的String对象

StringBuffer StringBuilder都是在原有对象上进行操作
> 所以 如果需要经常改变字符串的内容，建议使用这两个 

StringBuffer vs StringBuilder
StringBuffer是线程安全的
> StringBuffer 是线程安全的，StringBuilder是线程不安全的
> 线程不安全性能更高，所以在开发中有限使用StringBuilder
> StringBuilder>StringBuffer>String

什么是线程安全

1、多线程环境下

2、对这个对象方法不需要加入额外的同步控制

3、操作的数据的结果依然是正确的

StringBuffer的每个方法的源码 都加入了synchronized修饰符

优先StringBuilder > StringBuffer > String

什么情况下 线程不安全：

单线程情况下 不需要考虑 可以使用Stringbuilder

每次调用方法 创建一个栈针，

通常情况下 在service下的方法里使用StringBuilder

每个线程访问一个StringBuilder

这种情况下 是线程安全的 ，一个线程访问一个资源

HashMap ArrayList StringBuilder 都是线程不安全的 但是在Service中的方法中大量使用 它是没有问题的 不用考虑线程安全。

##### 6 接口和抽象类的区别

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


##### 7 递归必回题目
###### 7.1算法题：求N的阶乘

1、什么是递归
> 递归 就是方法内部调用方法自身
> 递归的注意事项
> - 找到规律，编写递归公司
> - 找出出口（边界值） ，让递归有结束边界
> 注意

```java
public static int getResult(int n){
    if(n<0){
        throw new ValidateException("非法参数");
    }
    if(n==1 || n==0){
        return 1;
    }
    return getResult(n-1) * n;
}
```

###### 7.2 斐波那契数列/不死神兔
数字规律：1,1,2,3,5,8,13

```java
public static int getFiBo(int n){
    if(n<0){
       return -1;
    }
    if(n==0){
        return 0;
    }
    if(n==1 || n==2){
        return 1;
    } else {
        return getFibo(n-1)+getFeiBo(n-2);
    }
}
```

##### 8 Integer int 缓存和自动装箱拆箱
JDK 1.5之后 自动装箱
Integer i=126 ;
Integer.valueOf(126);

缓存的高低之分
-128~127之间 从IntegerCache缓存中 读取；
IntegerCache.Low 默认值为-128
IntegerCache.HEIGHT 默认值 127

##### 9 重写和重载的区别
- 重载：发生在一个类里面，方法名相同，参数列表不同（混淆点：跟返回类型没有关系）
> 以下不构成重载
> public double add(int a,int b)
> public int add(int a,int b)
- 重写：发生在父类子类之间，方法名相同，参数列表相同

##### 10 List Set的区别 Collection Collections
###### 10.1 List Set的区别
- List 有序 可重复  ArrayList LinkedList
- Set 无序 不可重复 HashSet TreeSet 
  - 无序 不等于不可排序
  - 无序的意思：加进去的顺序 不等于输出的顺序 

###### 10.2 Collection Collections区别
Java工具类的命名 一般都是以+s结尾

- Collection 是一个集合接口
  Collection，提供了对集合对象进行基本操作的通用接口方法，所有集合都是它的子类，比如 List、Set 等。

- Collections 是一个包装类
  Collections，是一个工具类，它包含了很多静态方法，不能被实例化，比如排序方法： Collections. sort(list)等。


##### 11 ArrayList和LinkedList区别
