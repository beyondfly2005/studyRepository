单例设计模式的八种

饿汉式（静态常亮）

步骤如下

- 构造器私有化 防止new

- 本类内部创建对象实例
- 对外提供一个共有的静态方法，返回实例对象

```java
//饿汉式
class Sigleton{
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

##### 饿汉式（静态代码块）

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

###### 懒汉式（线程不安全）

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

###### 懒汉式（线程安装，同步方法）

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



懒汉式（线程安全，同步代码块）

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

###### 双重检查

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



###### 静态内部类

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

###### 枚举

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





经典单例模式

Runtime

但里面是注意事项和细节说明

单例模式保证了系统内存中该类

- 单例模式的使用场景：需要频繁



