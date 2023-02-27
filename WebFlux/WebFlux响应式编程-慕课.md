# WebFlux响应式编程

> 课程地址
> https://www.bilibili.com/video/BV16h411Z75F

### P1 概念

### P2 为什么哦使用函数式编程-1



### P3 为什么哦使用函数式编程-2



```java


int min2 = IntStream.of(nums).min().getAsInt();
System.out.println(min2);


int min2 = IntStream.of(nums).parallel().min().getAsInt(); //并行计算

```

```java
public static void main(String[] args){
    new Thread(new Runnable(){
       public void run(){
           System.out.println("ok");
       } 
    }).start();
    
    //jdk8
    new Thread(() -> {System.out.println("ok")});
}
```



### P4 lambda初接触



### P5 lambda初接触

```java
//有返回值的

interface Interface{
	int doubleNum(int i);
}

public class LambdaDemo1{
    
}
```



### P6 JDK8接口新特性



接口中只有一个要实现的方法

@FunctionInterface //函数接口  不要再加其他接口



单一职责 接口 设计原则



#### 新特性2

接口中的默认方法 ; (带默认实现的方法)

```java
default int add(int x,int y){
   return x+y;
}
```

