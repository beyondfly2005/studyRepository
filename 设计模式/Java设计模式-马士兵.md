> http://c.biancheng.net/view/1390.html

### GoF的23种设计模式

前面说明了 GoF 的 23 种设计模式的分类，现在对各个模式的功能进行介绍。

1. 单例（Singleton）模式：某个类只能生成一个实例，该类提供了一个全局访问点供外部获取该实例，其拓展是有限多例模式。
2. 原型（Prototype）模式：将一个对象作为原型，通过对其进行复制而克隆出多个和原型类似的新实例。
3. 工厂方法（Factory Method）模式：定义一个用于创建产品的接口，由子类决定生产什么产品。
4. 抽象工厂（AbstractFactory）模式：提供一个创建产品族的接口，其每个子类可以生产一系列相关的产品。
5. 建造者（Builder）模式：将一个复杂对象分解成多个相对简单的部分，然后根据不同需要分别创建它们，最后构建成该复杂对象。
6. 代理（Proxy）模式：为某对象提供一种代理以控制对该对象的访问。即客户端通过代理间接地访问该对象，从而限制、增强或修改该对象的一些特性。
7. 适配器（Adapter）模式：将一个类的接口转换成客户希望的另外一个接口，使得原本由于接口不兼容而不能一起工作的那些类能一起工作。
8. 桥接（Bridge）模式：将抽象与实现分离，使它们可以独立变化。它是用组合关系代替继承关系来实现，从而降低了抽象和实现这两个可变维度的耦合度。
9. 装饰（Decorator）模式：动态的给对象增加一些职责，即增加其额外的功能。
10. 外观（Facade）模式：为多个复杂的子系统提供一个一致的接口，使这些子系统更加容易被访问。
11. 享元（Flyweight）模式：运用共享技术来有效地支持大量细粒度对象的复用。
12. 组合（Composite）模式：将对象组合成树状层次结构，使用户对单个对象和组合对象具有一致的访问性。
13. 模板方法（TemplateMethod）模式：定义一个操作中的算法骨架，而将算法的一些步骤延迟到子类中，使得子类可以不改变该算法结构的情况下重定义该算法的某些特定步骤。
14. 策略（Strategy）模式：定义了一系列算法，并将每个算法封装起来，使它们可以相互替换，且算法的改变不会影响使用算法的客户。
15. 命令（Command）模式：将一个请求封装为一个对象，使发出请求的责任和执行请求的责任分割开。
16. 职责链（Chain of Responsibility）模式：把请求从链中的一个对象传到下一个对象，直到请求被响应为止。通过这种方式去除对象之间的耦合。
17. 状态（State）模式：允许一个对象在其内部状态发生改变时改变其行为能力。
18. 观察者（Observer）模式：多个对象间存在一对多关系，当一个对象发生改变时，把这种改变通知给其他多个对象，从而影响其他对象的行为。
19. 中介者（Mediator）模式：定义一个中介对象来简化原有对象之间的交互关系，降低系统中对象间的耦合度，使原有对象之间不必相互了解。
20. 迭代器（Iterator）模式：提供一种方法来顺序访问聚合对象中的一系列数据，而不暴露聚合对象的内部表示。
21. 访问者（Visitor）模式：在不改变集合元素的前提下，为一个集合中的每个元素提供多种访问方式，即每个元素有多个访问者对象访问。
22. 备忘录（Memento）模式：在不破坏封装性的前提下，获取并保存一个对象的内部状态，以便以后恢复它。
23. 解释器（Interpreter）模式：提供如何定义语言的文法，以及对语言句子的解释方法，即解释器。



### 1、单例模式Singleton

应用场景：

- 只需要有一个实例存在
- 比如各种Manager
  - PropertyManager
- 比如各种Factory

- 保证内存中只有一个实例存在

单例模式八种写法

###### 第一种饿汉式

```

```

spring bean工厂能够保证单例



### 2、策略模式Strategy

strategy [ˈstrætədʒi] 

Comparable 接口 java.lang.Comparable 

Comparator 接口 java.util.Comparator

Java1.8以后 接口里可以有方法实现，为了实现lamda表达式

开闭原则：对扩展开放，对修改关闭



比较大小的 应用不同的策略



比较器

Comparetor 

```java
public interface Comparetor<T>(){
   int compare(T o1,T o2);    
}


public void sort(T[] arr,Compartor<t> comparator){
    
}
```



多态设计模式的核心



策略模式 替代 if else  或者 替代 switch 封装了switch

lomoda 表达式

@FunctionlINterface

只有 方法

可以写方法的实现

defaultvoid m（）{

}

compares



Exten  可扩展性

```bash
class.forName().getInstance()  # jdk1.8之前

class.forName().getDeclareConstructor().newInstance(); #jdk1.9以后

```



学习的时候 过度的设计模式，工作用合适的设计模式？



#### 代理模式




### 3、工厂模式Factory

简单工厂

静态工厂

工厂方法

抽象工厂

Spring IOC 控制反转 或者依赖注入


在《设计模式》这本书中 没有简单工厂和静态工厂

任何可以产生对象的方法或类，都可以称之为工厂

单例也是一种工厂 getInstance() 有人称之为静态工厂

不可咬文嚼字，死抠概念

为什么有了new 之后，还要有工厂？

​	灵活控制生产过程

​	权限、修饰、日志... ...



```java
public class Main{
    Car c = new Car();
    c.go();
    Plane p =new P();
    p.go();
}
public class Car{
    public void go(){
        system.out.println("Car go wuwuwuwu....");
    }
}

public class Plane{
   public void go(){
       	system.out.println("plan flying sosososos....");
   }
}

public interface Movealbe(){
    public void go();
}
```





```
1、任意制定交通工具
	继承Moveable接口
2、任意制定生产过程
	XXXFactory.create()
3、任意定制产品一族
```

#### 简单工厂

```java
//简单工厂的可扩展性不好
public Car SimpleVehicleFactory(){
    public Car creteCar(){
		//前置处理 加权限
	    return new Car();
	}
}


```

#### 3.1 工厂方法

每一种产品做一个工厂

```java
public class Main{
    
}

public class CarFactory(){
    public Moveable create(){
        //前置操作
        //日志
        System.out.println("a car created!");
        return new Car();
    }
}
public class PlanFactory(){
    public Moveable create(){
         //日志
        System.out.println("a plan created!");
        return new Plan();
    }
}
    
```

#### 3.2 抽象工厂abstractFactory

```java
public abstract classVehicleFactory{
    
}


public class ModernFactory extends AbstractFactory{
    
}
public class MagincFactory extends AbstractFactory{
    
}
```

现代人：开汽车，吃面包 ，开AK47的枪

魔法人：骑扫帚，吃毒蘑菇，武器是魔法棒

```java
public abstract class AbstrctFactory{
    abstract Foot createFood();
    abstract Vehiclee createVehicle();
    abstract Weapon createWeapon();
}

public abstract classs Food{
   abstract void printName();
}
public abstract classs Vehicle{
    abstract void go();
}
public abstract classs Weapon{
    abstract void fire();
}
```



1、能不能用interface ，能不能用

​		食品  从语义上来讲， 是各种食物的泛指，用抽象更合适

​		可移动的：是

形容词用接口，名称用抽象类



##### 工厂方法vs抽象工厂

- 工厂方法：在各种单一产品扩展方便，汽车、AK47、帽子、 但是不方便在产品一族上扩展

- 抽象工厂：在产品一族 上容易扩展，但在单一某个产品上不容易扩展



坦克大战中 ，应用抽象工厂

默认的坦克 ，默认的子弹，默认的爆炸方式

另一种坦克， 另一种子弹 ，另一种爆炸方式或效果

##### SpringBen工厂

​	结合以上两种：抽象工厂+工厂方法



### 4、调停者模式Mediator 

消息中间件



### 5、门面Facade  外观模式



### 6、装饰器模式Decorator

需求：

坦克加一个外壳显示

加一个血条

加一条尾巴

子弹加一条尾巴

子弹加一个外壳



最简单的方式是 使用继承，不能解决 既加一个装饰条，又加一个血条，又加一个尾巴

装饰和被装饰者之间依赖太强

用聚合代替继承

子弹 的



### 7、责任链Chain Of Responsibility

场景：

在论坛中发表文章

后台要经过信息处理才能发布或者进入数据库



```
public class Main{
	ppublic static void (String[] args){
		Msg msg = new Msg();
		msg.setMsg("大家好<scriot></script>大家都是996吗")
		
		String r =msg.getMsg();
		r.r.replace('<','[');
		r.r.replace('>',']');
		
	}
}

class Msg{
	String name;
	String msg;
}
```

封装变化

```
Interface Filter{
	void doFilter();
}
class HTMLFilter implements{

}

class SensitiveFilter().doFilter();

//如果需要添加新的Filter
将新的Filter 串起来
```

```
class FilterChain{
	List<Filter> filters = new ArrayList<>();
	
	public void add(){
	
	}
	public void doFilter(){
	
	}
}
```

### 8、Observer观察者





### 9、Composite组合模式



### 10、Flyweight享元



### 11、Proxy静态代理与动态代理



### 12、Iterator迭代器



### 13、Visitor访问者



### 14、Builder构建器



### 15、Adapter适配器



### 16、Bridge桥接



### 17、Command命令

封装命令

别名：Action 动作/ Transaction事务



### 18、Prototype原型

- Java自带原型模式  Object.clone() 因此也叫克隆模式 

- 实现原型模式需要实现标记性接口Cloneable

- 一般会重写clonse()方法
   - 如果只是重写clone()方法，而没有实现接口，调用时会报异常。

- 一般用于一个对象的属性已经确定，需要产生很多相同对象的时候

- 需要区分深克隆和浅克隆





### 19、Memento备忘录

记录快照

存盘

### 20、TemplateMethod模板方法



### 21、State状态



### 22、Intepreter解释器





### 六大设计原则





