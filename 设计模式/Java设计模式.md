# 设计模式的七大原则

#### 单一职责

状态模式

if / else 添加状态时 需要对所有的代码进行修改

解释器设计模式

解释器设计模式的类图

设计模式的各个角色

SpelExpressionparser





7+1+1



JVM优化

多线程高并发

设计模式

Redis

ZooKeper

Mysql






#### 发布-订阅模式 与观察者模式的异同

> https://www.cnblogs.com/lovesong/p/5272752.html
>

1. 两者最大的区别是调度的地方不同。
虽然两种模式都存在订阅者和发布者（具体观察者可认为是订阅者、具体目标可认为是发布者），但是观察者模式是由具体目标调度的，而发布/订阅模式是统一由调度中心调的，所以观察者模式的订阅者与发布者之间是存在依赖的，而发布/订阅模式则不会。
  发布订阅模式 是有调度中心的 ，例如MQ 就是调度中心

2. 两种模式都可以用于松散耦合，改进代码管理和潜在的复用。


#### 观察者模式

> https://blog.csdn.net/m0_47944994/article/details/127903096

##### 观察者模式的四个角色
- Subject 抽象主题/抽象被观察者
- ConcreteSubject 具体主题/具体被观察者
- Observer 抽象主题
- ConcreteObserver 抽象被观察者

##### 观察者模式的代码实现(Java)

###### 
```java

//（抽象被观察者）接口, 让（具体观察者）WeatherData 来实现
public interface Subject {	
	public void registerObserver(Observer o);
	public void removeObserver(Observer o);
	public void notifyObservers();
}
```

