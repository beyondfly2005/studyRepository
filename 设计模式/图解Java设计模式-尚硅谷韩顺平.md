### 图解设计模式—尚硅谷韩顺平



> https://www.bilibili.com/video/BV1G4411c7N4



### P1、设计模式面试题（1）

原型设计模式





七大设计原则核心思想

单一职责

接口隔离

依赖倒转

里氏替换

开闭原则

迪米特法则

合成复用原则



### P2、设计模式面试题（2）



解释器设计模式

1、介绍解释器设计模式是什么

2、画出

3、请说明Spring框架





### P28、设计模式概述和分类

###### 掌握设计模式的层次

1、听说过

2、用了设计模式，但是自己却不知道

3、学过设计模式，也在使用，一些新的设计模式没学过

4、阅读源码或框架，在其中看到别人的设计模式

5、不自然的使用设计模式 并灵活运用设计模式

###### 设计模式介绍

1、设计模式

2、设计模式的本质

3、

4、设计模式并不局限于某种语言

###### 设计模式类型：

1、创建性模式：对象的创建 ：单例模式、抽象工厂模式、原型模式、建造者魔兽、工厂模式

2、结构模式：适配器模式、桥接模式、装饰、组合模式、外观模式、享元模式、代理模式

3、行为型模式：模板方法模式、命令模式、访问者模式、迭代器模式、观察者模式、中介者模式、备忘录模式、解释器模式、状态模式、策略模式、职责链模式(责任链模式)



### P29、单例模式

单例模式：采取一定的方法保证在整个





##### 单例设计模式的八种方式

1、饿汉式（静态常量）

2、饿汉式（静态代码块）

3、懒汉式（线程不安全）

4、蓝时尚（线程安全、同步方法）

5、懒汉式（线程安全、同步代码块）

6、双重检查

7、静态内部类

8、枚举



单例设计模式的八种

##### 1、饿汉式（静态常量）

步骤如下

- 构造器私有化 防止new

- 本类内部创建对象实例
- 对外提供一个共有的静态方法，返回实例对象

```java
//饿汉式
class Sigleton {
    // 1 构造器私有化，外部不能new
    private Singleton(){
        
    }
    //2 本类内部创建对象实例
    private final static Singleton instance = new Singleton();
   
    //3 对外提供一个共有的静态方法，返回实例对象
    public static Singleton getInstance(){
        return intance;
    }
}

//测试类
public class SingletonTest01 {
    //测试
    public static void main(String[] args){
        Singleton instance1 = Singleton.getInstance();
        Singleton instance2 = Singleton.getInstance();
        System.out.println(instance1 == instance2); //true 表明是同一个实例
        System.out.println("instance1.hashCode"+instance1.hashCode());
        System.out.println("instance2.hashCode"+instance1.hashCode());
    }
}
```

###### 优缺点

优点：这种写法比较简单， 就是在类装载的时候就完成了实例化，避免了线程同步问题

缺点：在类装载的时候就完成了实例化，没有达到Lazy Loading懒加载的效果，如果始终没有使用过这个实例，就会造成内存浪费

这种方式基于classloader机制避免了多线程同步问题，不过instance在类装载

结论：这种单利模式可用，可能造成内存浪费

##### 2、饿汉式（静态代码块）

```java
class Singleton{
	
    private Singleton(){
        
    }
    private static Singleton instance;
    
    static {
        instance = new Singleton();
    }
    
    public static Singleton getInstance(){
        return intance;
    }    
}

//测试类
public class SingletonTest01 {
    //测试
    public static void main(String[] args){
        Singleton instance1 = Singleton.getInstance();
        Singleton instance2 = Singleton.getInstance();
        System.out.println(instance1 == instance2); //true 表明是同一个实例
        System.out.println("instance1.hashCode"+instance1.hashCode());
        System.out.println("instance2.hashCode"+instance1.hashCode());
    }
}
```

###### 优缺点说明

- 这种方式和上面的范式其实类似，只不过类的实例化的过程放在了静态代码块中
- 结论：这种单利模式

##### 3、懒汉式（线程不安全）

```java
class Sington{
    private static Singleton instance;
    
    private Singleton(){
        
    }
    
    //静态共有方法，当使用时，才去创建instance
    public static Singleton getInstance(){
        if(instance == null){
            instance = new Singleton();
        }
        return instance;
    }
}

//测试类
public class SingletonTest01 {
    //测试
    public static void main(String[] args){
        System.out.println("懒汉式1, 线程不安全");        
        Singleton instance1 = Singleton.getInstance();
        Singleton instance2 = Singleton.getInstance();
        System.out.println(instance1 == instance2); //true 表明是同一个实例
        System.out.println("instance1.hashCode"+instance1.hashCode());
        System.out.println("instance2.hashCode"+instance1.hashCode());
    }
}

```

###### 优缺点：

- 起到了懒加载Lazy Loading的效果，但是只在单线程下使用
- 如果在多线程下，一个线程进入了if(singleton==null)判断语句块，还没来得及往下执行，还没有创建实例，零一二线程也通过了这个判断语句，这时就会产生多个实例，所以早多线程环境下不可以使用这种方式
- 结论：在实际的开发中，不要使用这种方式

##### 4、懒汉式（线程安装，同步方法）

```java
class Sington{
    private static Singleton instance;
    
    private Singleton(){
        
    }
    
    //静态共有方法，当使用时，才去创建instance
    //加入了synchronized 同步处理方法，多个线程同时创建实例，需要排队等待，不允许多个同时执行，只允许按顺序执行
    public static synchronized Singleton getInstance(){
        if(instance == null){
            instance = new Singleton();
        }
        return instance;
    }
}

//测试类
public class SingletonTest01 {
    //测试
    public static void main(String[] args){
        System.out.println("懒汉式1, 线程不安全");        
        Singleton instance1 = Singleton.getInstance();
        Singleton instance2 = Singleton.getInstance();
        System.out.println(instance1 == instance2); //true 表明是同一个实例
        System.out.println("instance1.hashCode"+instance1.hashCode());
        System.out.println("instance2.hashCode"+instance1.hashCode());
    }
}
```



###### 优缺点说明：

- 结局了线程不安全问题
- 效率太低，每个线程都想获得类的实例的时候，执行getInstance()



##### 5、懒汉式（线程安全，同步代码块）

```java
class Sington{
    private static Singleton instance;
    
    private Singleton(){
        
    }
    
    //静态共有方法，当使用时，才去创建instance
    //加入了synchronized 同步处理方法，多个线程同时创建实例，需要排队等待，不允许多个同时执行，只允许按顺序执行
    public static Singleton getInstance(){
        if(instance == null){
            synchronized (Singleton.class){
            	instance = new Singleton();
            }
        }
        return instance;
    }
}
```

###### 优缺点

- 这种方式  本意是想对第四种方式进行改进
- 但是这种同步不能起到线程同步的问题
- 结论：在实际开发中，不能使用这种方法。多线程环境中 实例会创建多个 线程不安全

##### 6、双重检查

```java
class Sington{
    private static volatile Singleton instance;
    // volatile 立即刷新到内存，
    private Singleton(){
        
    }
    
    //静态共有方法，当使用时，才去创建instance
    //加入双重检验方式 解决了线程安全问题，同时解决了懒加载问题
    public static synchronized Singleton getInstance(){
        if(instance == null){
            synchronized(Singleton.class){
                if(instance == null){
            		instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

//测试类
public class SingletonTest01 {
    //测试
    public static void main(String[] args){
        System.out.println("双重检查, 线程不安全");        
        Singleton instance1 = Singleton.getInstance();
        Singleton instance2 = Singleton.getInstance();
        System.out.println(instance1 == instance2); //true 表明是同一个实例
        System.out.println("instance1.hashCode"+instance1.hashCode());
        System.out.println("instance2.hashCode"+instance1.hashCode());
    }
}
```

###### 优缺点分析

- Double-Check概念是多线程开发中经常使用的，如代码中所示，我们进行了两次if(singleon==null)



##### 7、静态内部类

静态内部类特点：

- 当外部类被装载时，内部静态内部类并不会被立即装载
- 静态内部类在调用getInstance()方法时，会导致静态内部类被装载，并且只装载一次，而且在装载的时候 是线程安全的

```java
//使用静态内部类完成单例模式, 推荐使用
class Singleton{
    priate static volatile Singleton instance;
    
    private Singleton(){};
    
    private static lass SingletonInstance{
		private static final Singleton INSTANCE =  new Singleton();
    }
    
    //提供静态共有方法
    public static synchronized Singleton getInstance(){
        return SingletonInstaince.INSTANCE;
    }
}
```

###### 优缺点说明

- 这种方式采用了类装载的机制啦保证初始化实例时只有一个线程
- 静态内部类方式在Singleton类被装载时并不会被立即实例化，而是在需要实例化时，调用getInstance方法，才会被SingletonInstance类，从而

##### 8、枚举

```java
//通过枚举方式实现单例模式
enum Sinleton {
    INSTANCE;
    public void sayOK(){
        System.out.println("OK~");
    }
}

public class SingletonTest08{
    public static void main(String[] args){
        Singleton instance 
    }
}
```

优缺点：

- 借助JDK1.5中添加的枚举实现单利模式，不仅能避免多线程同步问题，而且还能钢制反序列化重新创建对象问题



### P37、单例模式JDK源码分析

1、JDK中，java.lang.Runtime就是经典单例模式

2、代码分析+Debug源码+代码说明

```java
public class Test{
	public static void mian(String[] args){
		Runtime r = Runtime.getRuntime();
	}
}

public class Runtime{
    private static Runtime currentRutime = new Runtime();
    public static Runtime getRuntime(){
        return currentRuntime;
    }
    private Runtime(){
        
    }
}
//用的单例模式的第一种写法饿汉式 线程安全
```

##### 单例模式注意事项和细节说明

1、单例模式保证了系统内存中该类值存在一个对象，节省了系统资源，对于一些需频繁创建销毁的对象，使用单例模式可用提高系统性能。

2、当向实例化一个单例类的时候，必须要记住使用响应的获取对象的方法，而不是使用new

3、单例模式的使用场景：需要频繁的进行创建和销毁的对象、创建对象时耗时过多或销毁资源过多（即：重量级对象），但又经常用到的对象，工具类对象、频繁访问数据库或文件的对象（比如数据源、session工厂等）



### P39、简单工厂模式





### P49、原型模式

###### 克隆羊问题

```java
public class Sheep{
	private String name;
    private int age;
    private String color;
    getter;
    setter;
}
public Client{
	public void main(String[] args){
        
    }
}
```

###### 传统方式解决克隆羊问题

1、

2、

3、

4、改进思路 Java中Object类是所有类的基类  Cloneable接口



### P50、原型模式-克隆羊

##### 原型模式基本介绍

1、

2、原型模式是一种创见性设计模式

3、



##### 原型模式-原理结构图（UML类图）



原理结构说明

Prototype:  原型类

ConcretePrototype



##### 原型模式解决克隆羊问题

```java
public Sheep implements Cloneable{
    
	
    //重写clone方法
	protected Object clone() throws CloneNotSupported{
    	super.clone();
	}
}
```

如果这只羊多了一个属性，代码只需要改一行



### P51、原型模式Spring代码分析

##### Spring框架中哪里用到了原型模式

1、Spring中原型bean的创建，就是原型模式

2、代码分析

beans.xml

```xml
<bean id="bean01" class="com.xxx.Monster" scope="protptype" />
```



```java
ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
Object bean =context.getBean("bean01");

Object bean2 =context.getBean("bean01");
System.out.println(bean==bean2);  //输出false
```

```
else if(mdb.isProtoType){
	
}
```

### P52、原型模式-浅拷贝

##### 浅拷贝的介绍





### P52、原型模式-深拷贝





### P58、建造者模式

##### 盖房项目需求

1、建房子：打地基、砌墙、封顶

2、房子各种各样

3、编写程序



##### 传统方式解决建房问题

```java
public abstract AbstractHouse{
    //打地基
    public abstract void buildBasic();
    //砌墙
    public abstract void buildWall();
    //封顶
    public abstract void roofed();
}
```

