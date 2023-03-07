# Java面试3000集 图灵

> 牛客网最热门Java面经整理-300集视频教程
> https://www.bilibili.com/video/BV1YN411F77D
> 
> 
### 1 MyBatisPlus 对比MyBatis有哪些优势
1. MyBatisPlus封装了一些常用的操作，可以使用简单的代码实现一些复杂的操作，减少了代码量。
2. MyBatisPlus提供了一次常用的CRUD操作，可以减少一些常规的操作代码
3. MyBatisPlus提供了一些高级功能 如分页、逻辑删除、多表查询、乐观锁等待 这些功能可以大大提高开销量
4. MybatisPlus提供了丰富的插件支持，可以方便的进行二进制缓存、性能分析、分页插件等等的基础
5. MybatisPlus可以通过配置文件或注解的方式进行配置，开发者可以根据需要选择合适的配置方式
6. MybatisPlus提供了代码生成器 对于增删改查代码 可以快速进行生成 减少开发

## 2 Java中有哪几种方式来创建新城执行任务
1 继承Thread类
```Java
public class Thread1 extends Thread{
    
}
```

2 实现Runnable接口
```java
public class Thread2 implements Runnable {
    
}
```

匿名内部类方式 实现
```java
```

函数式接口 Lamda表达式方式实现


3 实现Callable接口
```java

```
开启线程 执行任务，可以拿到执行结果，这是和Runnable接口的区别

Java 中类与类之间是单继承的
接口与接口之间 可以是多继承的

4 利用线程池 创建线程
Executors方式创建线程池

```java

```

底层都是基于runnable接口

### 3 为什么不建议使用Executors方式创建线程池
1 FixedThreadPool
```java

```

2 SingleThreadExecutor
当我们使用Executors创建
```java

```
可能造成OOM内存溢出， 不能自定义线程的名字，不利于排查问题


### 线程池有哪几种状态，每种状态分别代表什么

1 RUNNING
表示线程正常运行，技能接受新任务，也能正常处理队列中的任务

2 SHUNDOWN
不会接受新任务， 但是会继续吧对类中的任务处理完成

3 STOP
shutdown now() 方法时， 线程就进入STOP状态，不接受新任务，也不会处理队列中的任务

4 TIDYING
线程池中没有线程在运行后，线程池的状态就会编程TIDYING 并且会调用terminated(),该方法是空方法

4 TERMINATED
terminated()执行完成后

### Sychronized和ReentrantLock有哪些不同点


4 