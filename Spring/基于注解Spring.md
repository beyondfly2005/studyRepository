## Spring注解版

> 尚硅谷 Spring注解版
>
> https://www.bilibili.com/video/av243820106/?p=2



### P01 课程简介-注解驱动开发





### P02 组件注册-@Configuration&@Bean给容器中注册组件



如何注册一个类 Person

```java
@Date
@AllArgsConst
@NoArgsConstr
class Person {
    
}
```

```xml
<bean>
    
</bean>

```



```java
public class MainTest{
	public static void main(String[] args){
        ApplicationContext ctx = new ClassPathXmlApplication();
        Person bean = (Person)ctx.getBean("person");
        System.out.println(bean);
    }
}

```

```java
@Configuration //配置类=配置文件
public class MainConfig{
    
    @Bean("person") //
	public Person person(){
        return new Person("lisi",20);''
    }
}


```

```java
public class MainTest{
	public static void main(String[] args){
        ApplicationContext ctx = new AnnotationConfigApplicaion(MainConfig.class);
        Person bean = (Person)ctx.getBean(Person.class);
        System.out.println(bean);
        
        String[] nameFOrTYpe = applicationContext.getBeanNameForType(Person.class);
        
    }
}
```



### P03、组件注册-@ComponentScan-自动扫描组件&指定扫描规则

### P04、组件注册-自定义TypeFilter指定过滤规则