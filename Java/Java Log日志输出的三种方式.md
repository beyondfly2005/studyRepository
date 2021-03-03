### Java Log日志输出的三种方式



#### 1、我们最常用的方式 System.out.println()

```java
try{
    //xxx
} catch(Exception e) {
    System.out.println(e.getMessage());
}
```

**缺点**：	不能按日志级别进行输出日志

#### 2、slf4j方式输出

```java
private static Logger logger = LoggerFactory.getLogger(XXXXX.class);

try {
	//
} catch (Exception e) {
	logger.error(e.getMessage(),e);
}
```

**缺点：**需要声明一天个静态变量，需要引入

#### 3、使用@Slf4j注解  (推荐使用)

```java
import lombok.extern.slf4j.Slf4j;

@Slf4j  //第1步在类上 添加注解
public class Test{
    
    public String hello(String username){
        log.info(username); //直接使用log的方法 进行不同等级的日志输出
        log.warn(username);
        log.error(username);
    }
}
```

需要注意的是：@Slf4j 这个注解需要 引入lombok 的依赖

```xml
        <!-- lombok 插件 -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
            <scope>provided</scope>
        </dependency>
```

