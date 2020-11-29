### 设计模式系列课程 

> https://www.bilibili.com/video/BV1Zt411d7bi?p=1



### 1、简单工厂

#### 1.1 什么是简单工厂

简单工厂模式属于类的创建型模式，又叫静态工厂方法模式。

通过转码定义一个类来负责创建其他类的实例，被创建的实例通常都有共同的父类。

#### 1.2 工厂模式中包含的角色及其职责

###### 1、工厂(Creator)角色

简单工厂模式的核心，他负责实现创建所有实例的内部逻辑，工厂类可以被外界直接调用，创建所需的产品对象。

###### 2、抽象(Product)角色

简单工厂模式所创建的所有对象的父类，他负责描述所有实例所共有的公共接口。

###### 3、具体产品(Concrete Product)角色

简单工厂模式所创建的而具体实例对象

#### 1.3 简单工厂模式的优缺点

在这个模式中，工厂是真个模式的关键所在，它包含必要的判断逻辑，能够根据外部给定的信息，绝对究竟应该创建那个具体的对象，用户在使用时可以根据工厂类去创建所需的实例，而无需了解这些对象是如何创建以及如何组织的，有利于整个软件体系结构的优化。

不难发现，简单工厂模式额缺点也体现在工厂类上，由于工厂类集中了所有的实例的创建逻辑，所以“高内聚”方面做得并不好。另外，当系统中的具体产品类不断增多时，可能会出现要求工厂类也要做出相应的修改，扩展性并不好。

#### 1.4 简单工厂的应用实例

数据库jdbc的连接就是才用的简单工厂

可以连接mysql oracle sqlserver 返回都是jdbc连接对象，使用Class.forName() 加载驱动

```java
public class main{
    Apple apple = new Apple();
    apple.get();
    Banana banana = new Banana();
    banana.get();  
}
```

```java
//采摘
public class Apple{
    public void get(){
    
    }
}
```

```java
public class Banana{
    public void get(){
        
    }
}
```

抽取水果接口

```java
public class FruitFactory{
    //v1
    public Fruit getApple(){
        return new Apple();
    }
    public Fruit getBanana(){
        return new Banana();
    }
    //v2 改为静态方法 方便调用
    public static Fruit getApple(){
        return new Apple();
    }
    public static Fruit getBanana(){
        return new Banana();
    }
}
```

进一步改进

```java
public class FruitFactory{
    public Fruit getFruit(String type){
        if(type.equalsIgnore("apple")){
            //return new Apple();
            return Apple.class().newInstance();
        } else if(type.equalsIgnore("banana")){
            //return new Apple();
            return Apple.class().newInstance();
        }
    }
}
```

再次改进（使用类的加载器，反射）

```java
public class FruitFactory{
    public Fruit getFruit(String type){
        Class fruit =Class,forNmae(type);
        return (Fruit)fruit.getInstance();
     }
}        
```

### 2、工厂方法

#### 2.1 什么是工厂方法模式

工厂方法模式同样属于类的创建型模式，又称多态工厂模式。

工厂方法模式的意义是定义一个创建产品对象的工厂接口，将实际创建工作，推迟到子类当中，核心工厂类不再负责产品的创建，这样核心类成为一个抽象工厂角色， 仅负责具体工厂子类必须实现的接口，这样进一步抽象画的好处是使得工厂方法模式可用使系统在不修改具体工厂角色的情况下，引进新的产品。

例子：增加一种水果比如梨子 需要同时修改工厂类 

#### 2.2 模式中包含的角色及其职责

1、抽象工厂(Creator)角色

工厂方法模式的核心，任何工厂类都必须实现这个接口

2、具体工厂(Concrete Creator)角色

具体工厂类时抽象工厂的一个实现，负责实例化产品对象

3、抽象(Product)角色

工厂方法模式所创建的所有对象的父类，它负责描述所有实例所共有的公共接口

4、具体产品(Concrete Product)角色

工厂方法模式所创建的具体实例对象

#### 2.3 Java代码示例

```java
public abstract class FruitFactory {
    public abstract Fruit getFruit();
}

public class AppleFactory extends FruitFactory{
    @Override
    public Fruit getFruit() {
        return new Apple();
    }
}

public class BananaFactory extends FruitFactory {
    @Override
    public Fruit getFruit() {
        return new Banana();
    }
}

//客户端类
public class MainClass {

    public static void main(String[] args) {
        FruitFactory appleFactory = new AppleFactory();
        Fruit apple = appleFactory.getFruit();
        apple.get();

        FruitFactory bananaFactory = new BananaFactory();
        Fruit banana = bananaFactory.getFruit();
        banana.get();
    }
}

//如果需要新增一个水果类型 创建一个新的工厂即可 无需改动之前的代码
public class PearFactory extends FruitFactory{
    @Override
    public Fruit getFruit() {
        return new Pear();
    }
}

//客户端类增加梨子工厂 无需改动之前的代码
public class MainClass {

    public static void main(String[] args) {
        FruitFactory appleFactory = new AppleFactory();
        Fruit apple = appleFactory.getFruit();
        apple.get();

        FruitFactory bananaFactory = new BananaFactory();
        Fruit banana = bananaFactory.getFruit();
        banana.get();

        //如果需加入一个新的水果梨子
        FruitFactory pearFactory = new PearFactory();
        Fruit pear = pearFactory.getFruit();
        pear.get();
    }
}

```

#### 2.4 工厂方模式和简单工厂模式比较

工厂方模式与简单工厂模式在结构上的而不同不是很明显。工厂方法了的核心是一个抽象工厂类，而简单工厂模式把核心放到了一个具体实现类上。

工厂方法模式之所以有一个别名叫多台性工程模式是因为具体工厂类都有共同的接口，或者共同的抽象父类

当系统扩展需要添加新的产品对象时，紧急需要添加一个具体对象以及一个具体工厂对象，原有工厂对象不需要进行任何修改，也不需要修改客户端（-加），很好的符合“开闭”原则，而简单工厂模式在添加新产品对象后不得不修改工厂方法，扩展性不好。

工厂方法模式退化后可以演变为简单工厂模式。

### 3、抽象工厂模式

#### 3.1 什么是抽象工厂模式

抽象工厂模式是所有形态的工厂模式中最为抽象和最具一般性的。抽象工厂模式可以向客户端提供一个接口，使得客户端不必指定产品的具体类型的情况下，能够创建多个产品族的产品对象。

