>视频地址 ：https://www.bilibili.com/video/BV185411477k

### 课程内容

Spring IOC工厂

SpringAOP编程

​	Spring动态代理 底层实现

3 持续集成

事务处理

MVC框架集成

注解编程



### 学习建议

- 认真听
- 多练、多想、多问
- 坚持



### 简介

全程 SpringFramework

《J2EE设计和开发》EJB架构的缺陷,提出了Interface21

《J2EE Development Without EJB》 提出轻量级JEE开发解决方案Spring

## 第一章 引言

### 1. EJB存在的问题

##### EJB框架的缺陷

Enterprise Java Bean 企业级JavaBean

- 运行环境苛刻
- 代码移植性差



Tomcat 

- Servlet引擎
- 称为WebServer

EJB

- 运行在EJB容器
- 例如Weblogic  ApplicationServer  收费 Oracle
- WebSphere ApplicationServer 收费 IBM

- 移植性差  基于WebLogic中的代码无法运行在WebLogic中运行
- 重量级框架

### 2. 什么是Spring

```
Spring是一个轻量级的JavaEE解决方案，整合众多优秀的设计
```

- 轻量级

```
1\
```

Java EE 分层开发

```
Controller	struts2
Service		
Dao			Mybatis	
DB

---------------------
SpringMVC 	Controller
Spring		Service

```

- 整合设计模式


```
1、工厂设计模式
2、代理设计模式
3、模板设计模式
4、策略设计模式
```

###   3. 设计模式

```
1、广义概念
面向对象设计中，解决特定问题的经典代码
2、狭义概念
GOF 4人帮定义的23种设计模式：
工厂 适配器 装饰器 门面 带哦你 模板.....
```

### 4. 工厂设计模式

```
1、概念：通过工厂类，创建对象
通过new 创建对象

2、好处：解耦合
	耦合：代码直接的强关联关系，一方的改变会影响到另一方
	问题：不利于代码维护
	简单理解：把接口的实现类、硬编码在程序中
	
```

```java
public class ServlerActionControllerMain{
    
}
```



```java
public void test(){
    UserService userService = new UserServiceImpl();
    //如果改为
    //UserService userService = new UserServiceImplNew();
    //通过工厂类的工厂方法解耦
    //调用者角度 耦合解决了
    USerService userService = BeanFactory.getUserService();
    userService.login(username,password);
    User user = new User(username,password);
    userService.registry(user);
}

//##### 工厂类
public class BeanFactory(){
	//工厂类中还存在耦合
    //需要解决工厂类工厂方法中耦合
    /**
	    采用new 构造方法 会制耦合
	    对象的创建方法
	    1、通过构造方法创建对象 UserService userService = new UserService();
	    2、通过反射方式创建对象，解耦合
	    Class class=Class.forName("com.beyondsoft.basic.UserServiceImpl");
	    UserService userService=(UserService)class.newInstance();
    */
    public static UserService getUserService()}{
    	
	    private static Properties env = new Properties();
    	//文件内容读尽量  IO是系统级资源，尽量避免重复打开IO，最好在程序启动时一次性读取 因此使用静态代码块来实现
    	static{
            try{
                //第一步 获取IO输入流
                InputSteam inptStream=BenFactory.class.getResourceAsStream("/application.properties")
                //第二步 将文件内容封装到Properties集合中 key=userService value=com.beyondsoft.basic.UserServiceImpl
                evn.load(inputStream);
                inputStream.close();
            } catch(IOException e){
                e.printStackTrace();
            }
        }
    
    	//return new UserServiceImpl();
    	//改为
	    UserService userService = null;
        try{
            						//com.beyondsoft.basic.UserServiceImplNew 还存在耦合 通过配置文件来解决 比如properties 
            //Class class=Class.forName("com.beyondsoft.basic.UserServiceImpl");
            //改为
            Class clazz=Class.forName(env.getProperty("userService"));
            userService=(UserService)class.newInstance();
        } catch (ClassNotFoundException e){
        } catch (InstantiactionException e){
            e.printStackTrace();
        } catch (IllegalAccessException e){
            e.printStackTrace();
        }
	    reurn userService;
	}
    

	public static UserDao getUserDAO(){
        UserDao userDao = null;
        try{            
            Class clazz = Class.forName(env.getProperty("userDao"));
            UserDao userDao=(UserDao)clazz.newInstance();
        } catch (ClassNotFoundException e){
            e.printStackTrace();
        } catch (InstantiactionException e){
            e.printStackTrace();
        } catch (IllegalAccessException e){
            e.printStackTrace();
        }
        return userDao;
    }
}

//#### resources 下applicationContext.properties
/*
properties 是字符串类型的键值对 
Properties类型的集合存储Properties的文件内容
Properties类型的集合特点：是个特殊的Map key=String Value=String类型
Properties[userService=beyondsoft.basic.UserServiceImp]
Properties.getProperty("userService")
*/
userService=com.beyondsoft.basic.UserServiceImpl
userDao=com.beyondsoft.basc.UserDaoImpl;

```

- 反射+配置文件 解耦

  如果改为一个新的实现类UserServiceImplNew

  只需要修改配置文件即可

#### 4.2 简单工厂



简单工厂存在的问题

- 结构相同
- 代码冗余：为每一个对象都设计了一个独立的的工厂方法

#### 4.3 通用工厂的设计

- 设计一个童工 提供一个工厂方法，帮我们生产我们需要的一切对象

```bas
方法定义
	两部分：方法声明+方式实现

方法声明：
	五个要素：修饰符 返回值类型 方法，名 参数表 异常(可选)
方法实现：
```

- 通用工厂的代码

```java
public static Object getBean(String key){  //key为properties中的key
    
    Object ret = null;
    try{
        Class clazz=Class.forName(en.getProperty(key));
        ret = clazz.newInstance();
    } catch(Exceptoion e){
        e.printStackTrace();
    }
    return ret;
}
```

### 4  通用工厂的使用方式

```properties
1、定义类型（类）
2、配置文件的配置告知工厂（applicationContext.properties）
	key = value
3、通过工厂获得类的对象
	Object ret=	BeanFactory.getBean("key")
	
```

```java
public Class Person{
    
}

person=com.beyondsoft.basic.Person

Person person=(Person) BeanFactory.getBean("persopn");
```

#### 5 总结

```
Spring本质：工厂 ApplicatonContext(ApplicationContext.xml)

```



## 第二章、第一个Spring程序

### 1 软件版本

```properties
JDK 1.8+
Maven3.5+
IDEA2018+
SpringFramework 5.1.4 
	#官方www.spring.io

# Maven3.6+IDEA2019 配合存在bug

```

### 2 环境搭建

- Spring的 Jar包

```properties
# 设置pom依赖
<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.1.4.RELEASE</version>
</dependency>

```

- Spring的配置文件

```
1、配置文件的放置位置：任意位置 没有印象要求
2、配置文件的命名：没有硬性要求 建议：applicationContext.xml

思考：配置文件位置、命名都是随意的，那么spring框架如何知道配置文件在哪，叫什么名字？
日后应用Spring框架时，需要进行配置文件路径的设置
```

- Idea 创建配置文件

```
建议在resources中添加配置文件
new -> XML Configuration FIle -> Spring Config
applicationContext.xml
```

### 3 Spring的核心API

Struts2核心类型：Action ActionSupport ServletActionContext

Mybatis核心类型：SqlSessionFactory SqlSession

- ApplicationContext

```
作用：
	Spring提供的ApplicationContext这个工厂，作用是对象的创建
好处：
	解耦合
```

- ApplicationContext 是一个接口类型
    - 设计为接口类型：屏蔽实现的差异
    - 非web环境工厂：ClassPathXmlApplicationContext (main函数 junit测试中使用)
    - web环境工厂：XmlWebApplicationContext

- ApplicationContext是重量资源

```
ApplicationContext工厂的对象占用大量内存
不会频繁的创建对象：一个应用只会创建一个工厂对象
ApplicationContext工厂 是线程安全的（可以被多线程并发访问）


```



### 4 程序开发

```java
1、 创建对象类型
2、配置文件的配置applicationContext.xml
3、通过工厂类 获得对象
    Spring工厂类ApplicationContext
	如果是用Junit测试，非web环境 使用其实现类ClasPathXmlApplicationContext

    
    
1> 使用Persion类型    
2> 配置applicationContext.xml
```

```
#配置applicationContext.xml

<bean id="" class=""/>
id 属性 起个名字 特点：唯一不能重复
class属性 配置全限定名 类的全名包含package包路径

<bean id="person" class="com.beyondsoft.basic.Person"/>
```

```java

@Test
public void test3(){
    //1 获得Spring的工厂
    ApplicationContext ctx = new ClassPathXmlApplicationContext("/applicationContext.xml");
    //2 通过工厂类获得对象
    Person person = (Person)ctx.getBean("person");
    //soutv 快捷键
    System.out.println("person="+person);
}

```

### 5 细节分析

- 名词解释

```
Spring工厂创建的对象 叫bean 或者组件componet
```

- Spring工厂的一些相关方法

```java
@Test
public void test3(){
    //1 获得Spring的工厂
    ApplicationContext ctx = new ClassPathXmlApplicationContext("/applicationContext.xml");
    //2 通过工厂类获得对象
    Person person = (Person)ctx.getBean("person");
    
    //通过这种方式获得队形，就不需要强制类型转换
    Person person = ctx.getBean("person",Person.class);	
    
    //当前spring配置文件 有且只能有一个bean标签是Person类型
    Person person = ctx.getBean(Person.class);	
    //soutv 快捷键
    System.out.println("person="+person);
    
    //获取的是Spring工厂配置文件中的所有bean标签的id值 person person1
    String[] beanDefinitonNames = ctx.getBeanDefinitionNames();
    for(){
        
    }
   	//根据类型获得spring配置文件中的id值
    String[] beanDefinitonNames = ctx.getBeanNamesForType(Person.class)
    for(){
        
    }
    
    //用于判断是否存在指定id的bean
    boolean flag = ctx.containsBeanDefinition();
    System.out.println(flag);
	
    //用于判断是否存在指定id的bean
    boolean flag = ctx.containsBean();
    System.out.println(flag);
}
```

- 配置文件中需要注意的细节

```markdown
1、只配置class属性
<bean class=""/>

A> 上述这种配置有没有id值? Spring会不会自动给生成默认值
	会 命名规则 com.beyondsoft.basic.Persion#0
B> 应用场景是什么？
	如果这个bean只需要使用一次，那么久可以省略id值
	如果这个bean会被使用多次，或者被其他bean引用则需要设置id值
2、name属性
作用：用于在Spring的配置文件中，为bean对象定义别名(小名)
<bean id="pserson" name="p" class="com.beyondsoft.basic.Persion"/>

```

```java
@Test
public void test5(){
    AppliationContex ctx = new ClassPathXmlApplicationContext("/applicationContext.xml");
    String
}
```

### 6. Spring工厂的底层实现原理（简易版）

```
1、Spring框架
通过ClassPathXmlApplicationContext工厂读取配置文件applicationContext.xml
2、Spring框架
获取bean标签的相爱难改观信息
通过发射创建对象
Class<?> clazz = Class.forName(class的值);
id的值=  clazz.newInstance();
等效于new 创建对象
Account account= new Account();
```

通过反射创建对象是否调用构造方法？如何来证明

会调用构造方法，并且调用的是无参构造

可以通过创建一个对象，在对象中的无参构造方法中 输出打印语句，来测试

- ```java
  public class Person(){
      public Person(){
          System.out.println("Person.person");
      }
  }
  
  @Test
  public void test6(){
      AppliationContex ctx = new ClassPathXmlApplicationContext("/applicationContext.xml");
      Person p = (Person)ctx.getBean("person");
      System.out.println("p = "+p);
  }
  
  // Ctl+Alt+F10 执行
  如果没有无参构造，只有有参构造怎么办？
  如果无参构造是私有的，Spring能否调用这个构造方法
  public class Person(){
      private Person(){
          System.out.println("Person.person");
      }
  }    
  使用反射可以调用对象的私有的构造方法创建对象    
  ```

**Spring工厂是可以调用 私有的方法创建对象 **



### 7. 思考

```
问题：未来在开发中，是不是所有的对象，都会交给Spring工厂来创建呢？
回答：理论上 是的，但是有特例：实体对象（Entity）
	实体封装数据库中表的数据，这些数据 通常由持久层框架来创建。
	例如：mybatis JPA JDBC Hibernate等
```



## 第三章、Spring5.x与日志框架的整合

```
Spring与日志框架进行整合，日志框架就可以在控制太重，输出Spring框架运行过程中的一些重要信息；
好处：方便了解Spring框架的运行过程，有利于程序的调试
```

- Spring如何整合日志框架

```
# 默认
Spring 1 .2 .3 早起都是与commons-logging.jar 进行整合
Spring5.x默认整合的日志框架logback log4j2

Spring 5.x整合log4j
1、引入loj4j jar包
2、引入log4j.properties

```

  - pom

```
<dependency>
	<groupId>org.sl4j</groupId>
	<artofactOd>slf4j-log4j12<artifactId>
	<version>1.7.25</version>
</dependency>
<dependency>
	<groupId>log4j</groupId>
	<artofactOd>log4<artifactId>
	<version>1.2.17</version>
</dependency>
```

- log4j.properties

```properties
#resources文件夹根目录
### 配置根
log4j.rootLogger= debug,console

### 日志输出到控制台显示
log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.Target=System.out
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layoutConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n
```



## 第四章、注入（injection）

### 1. 什么是注入

```

```

#### 1.1 为什么需要注入

**通过编码的方式，为成员变量赋值，存在耦合**

```java
@Test
public void test7(){
    AppliationContex ctx = new ClassPathXmlApplicationContext("/applicationContext.xml");
    Person p = (Person)ctx.getBean("person");
}
```

#### 1.2 如何进行注入[开发步骤]

- 类的成员变量提供set get方法

- 配置spring的配置文件

  ```
  <bean id="Person">
  	<property name="id">
  		<value>10</value>
  	</property>
  		<property name="name">
  		<value>xiaojr</value>
  	</property>
  </bean>	
  ```

  

#### 1.3 注入的好处

```
解耦合
```



### 2. Spring注入元分析（简易版）

**Spring会通过底层调用对象属性的set方法，完成成员变量的赋值，这种方式我们也称之为set注入**

```
Spring注入的工作原理
1、解析bean标签 使用反射创建对象
2、property标签 调用属性的set方法 进行赋值
```

## 第五章、Set注入详解