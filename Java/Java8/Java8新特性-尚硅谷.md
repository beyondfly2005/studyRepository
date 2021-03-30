## Java8 新特性

> 尚硅谷 - 李贺飞
>
> 视频地址：https://www.bilibili.com/video/BV14W411u7Ly?t=1

## P1、Java8新特性_简介

### 主要内容

**1、Lambda表达式**

2、函数式编程

3、方法引用与构造器引用

**4、Stream API**

​		操作Java中的数据如同操作SQL语句一样简单

5、接口中的默认方法与静态方法

6、新时间日期API

7、其他新特性

### Java 8 新特性简介

- 速度更快
- 代码更少（增加了新的语法Lambda表达式）
- 强大的Stream API
- 便于并行
- 最大化减少空指针异常Optional



**Java8种对于HashMap的改变**

**Jdk1.7  数组+链表**

HashMap 哈希表 哈希算法 用数组存储

不同Hash值，相同Hash值 进行equals比较

如果equals不同，则发生了碰撞，需要使用链表，后加的元素 添加在前面 ，指向 原来的元素，形成链表



**JDK1.8 数组+链表+红黑树**

当 链表中/(发生碰撞的数量)也就是链表的大小大于8 

并且总容量大于64的时候 将链表转为红黑树

红黑树 除了添加以外，其他操作都比链表块



HashMap HashSet

不知道用哪种表的时候 就用Hash表



**ConcurrentHashMap**

隔离级别/并发级别，默认是16

锁分段机制，concurrentLevel=16



JDK1.8之后 就不使用段，无锁算法，无锁添加



#### 内存结构的更新

底层的内存结构也不一样

JDK17 栈 堆  方法区 （方法区是 堆中 永久区PremGen的一部分）

画图的时候 要 画在 堆的下面

永久区中的内容 几乎不会被垃圾回收机制回收

会被垃圾回收机制回收，但是条件比较苛刻

JVM厂商 tr

Oracle-SUM - Hostpot 

Oracle JRocket

IBM J9 JVM

Taobao JVM 国产JVM

在JDK1.7以前，除了Hotspot 早就没有永久区了，

已经把方法区从永久区剥离出来，

Hotspot也从永久区中剥离出来

JDK 1.8 之后  Hotspot 也

MetaSpace 原空间，用的物理内存，物理内存多大，元空间就有多大

当方法区快要满的时候，或者容量过大的时候，垃圾回收机制才会计算回收

MetaSpace元空间 使用的物理内存 发生垃圾回收的概率非常低

OOM = Out Of Memery 内存溢出的 发生的概率 很低

Jvm调优参数

PermGenSize  无效了

MaxPremGenSize 无效

取而代之的

MetaspaceSize 

MaxMetaSpaceSize



**便于并行**

合并框架 - Fork/Join 自己写算法，写起来难 容易出错

JDK1.8 以后 简单



**最大化减少空指针异长 Optional容器类**

其中最为核心的为Lambda表达式和Stream API



## P2、尚硅谷_Java8新特性_为什么使用 Lambda 表达式

#### Lambda表达式

Lamdba是一个匿名函数（方法），我们可以把Lambda表达式理解为是一段可以传递的代码（将代码像数据一样进行传递）。可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了提升。



```java
package com.beyondsoft.java8;

public class TestLambda{
    
    //原来的匿名内部类
    @Test
    public void test1(){
        //匿名内部类
        Comparator<Integer> com = new Comparator<Integer>(){
            @Override
            public int compare(Integer o1,Integer o2){
                return Integer.compare(o1,o2);
            }
        }
        
        TreeSet<Integer> ts = new TreeSet<>(com);
    }
    
    //Lambda 表单式
    @Test
    public void test2(){
        Comparator<Integer> com = (x,y) -> Integer.compare(x,y);
        TreeSet<Integer> ts = new TreeSet<>(com);
    }
    
    //需求：获取当前公司中 年龄大于35的员工信息
    List<Employee> employees = Arrays.asList(
        new Employee("张三"，19,9999.99),
        new Employee("张三"，19,9999.99),
        new Employee("张三"，19,9999.99),
        new Employee("张三"，19,9999.99)
    );
    
    @Test
    public void test3(){
        List<Employee> list = filterEmployees(this.employees);
        for (Employee employee:list) {
            System.out.println(employee.toString());
        }
    }
    
    //需求：获取当前公司中 年龄大于35的员工信息
    public List<Employee> filterEmployees(List<Employee> list){
        List<Employee> emps = new ArrayList<>();
        for(Employee emp: list){
            if(emp.getAge()>35){
                emps.add(emp);
            }
        }
        return emps;
    }
    
    //需求：获取当前公司中 工资大于5000的员工信息
    public List<Employee> filterEmployees2(List<Employee> list){
        List<Employee> emps = new ArrayList<>();
        for(Employee emp: list){
            if(emp.getSalary()>5000){
                emps.add(emp);
            }
        }
        return emps;
    }
    
    //原来最好的方法 设计模式
    
    //优化1 设计模式 策略设计模式 给他什么策略 就按什么策略执行
    @Test
    public void test4(){
        List<Employee> list = filterEmployee(employees, new FilterEmployeeByAge());
        for (Employee employee: list) {
            System.out.println(employee);
        }
        System.out.println("------------------------");
        List<Employee> list2 = filterEmployee(employees, new FilterEmployeeBySalary());
        for (Employee employee: list2) {
            System.out.println(employee);
        }
    }
        
    //优化2 匿名内部类
    public void test5(){
        
    }
    
    //优化3 匿名内部类 改为lambda表达式
    @Test
    public void test6(){
        List<Employee> list = filterEMployee(employees,(e)-> e.getSalary <=5000);
        list.forEach(System.out::println);
    }
    
    //优化方式4 
    @Test
    public void test7(){
        employees.steam().filter((e)->e.getSalary()5000)
            .limit(2) //取前两个
            .forEach(System.out::println);
        
        
        employees.steam().map(Employy::getName)
            .forEach(System.out::println);
    }
}




// 员工对象
publicclass Employee{
    private 
}
    
```

## P3、Lambda 基础语法

###### Lambda表达式

```java
/**
一、Lambda表达式的基础语法
箭头操作符 或Lambda操作符
将表达式 拆分为两部分
左侧：Lambda表达式 参数列表
右侧：Lambda表达式中所需执行的功能  即Lambda体

语法一：无参 无返回值
  () ->System.out.print("Hello Lambda!");
语法二： 有一个参 并且无返回值
  (x) -> System.out.pring(x)
语法三： 只有一个参数 小括号可以省略不写
   x  -> System.out.println(x)
语法四：    
*/

public class TestLambda2{
    
    @Test void test1(){
        final int num =0 ;// jdk1.7 必须就是final jdk1.8 可以不显示声明式final 但是也是final的
        int num =0; //jdk1.7 必须是final jdk1.8 可以不用加 final 但是还是final的 
        Runnable r = new Runnable(){
            @Override
            public void run(){
                //System.out.println("Hello World!"+ num++);
                System.out.println("Hello World!"+ num);
            }
        };
        r.run();
        
        System.out.println("----------------");
        Runnable r1 = () -> System.out.pringln("Hello Lambda!");
        //Runnable r1 = () -> System.out.pringln("Hello Lambda!"+ num++); //final 类型 不能改变
    }

    
    @Test
}
```



## P15 Optional类

尽量避免空指针异常

Optional<T> 类（java.util.Optional）

```java

public class TestOptional{
    
    @Test
    public void test1(){
        Optional<Employee> op = Optional.of(new Employee());
    }
}
```



## P21 可重复注解

重复注解



定义重复注解 必须先订阅容器

用@Repeatable修饰下 



配合反射来使用

```java
@Test
public void test1() throws Exception{
    Class<TestAnnotation> clazz = test1.class;
    Method m1 = clazz.getMethod("show");
    
    MyAnnotation[] ma  m1.getAnntationsByType(MyAnnotation.class);
    for(MyAnnotation myAnnotation : mas){
        System.out.println(myAnnotation.class);
    }
}
```



@Target({TYPE,})