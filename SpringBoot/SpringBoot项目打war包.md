#### 一、修改pom.xml文件将默认的jar方式改为war：

```xml

<groupId>com.example</groupId>
<artifactId>application</artifactId>
<version>0.0.1-SNAPSHOT</version>
<!--默认为jar方式-->
<!--<packaging>jar</packaging>-->
<!--改为war方式-->
<packaging>war</packaging>

```

#### 二、排除内置的Tomcat容器（两种方式都可）：

##### 1.排除spring-boot-starter-web中的Tomcat

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

2. ##### 添加依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <!--打包的时候可以不用包进去，别的设施会提供。事实上该依赖理论上可以参与编译，测试，运行等周期。
        相当于compile，但是打包阶段做了exclude操作-->
    <scope>provided</scope>
</dependency>
```



#### 三、继承org.springframework.boot.web.servlet.support.SpringBootServletInitializer，实现configure方法

```
为什么继承该类，SpringBootServletInitializer源码注释：

Note that a WebApplicationInitializer is only needed if you are building a war file and deploying it. 

If you prefer to run an embedded web server then you won't need this at all.

注意，如果您正在构建WAR文件并部署它，则需要WebApplicationInitializer。
如果你喜欢运行一个嵌入式Web服务器，那么你根本不需要这个。
```

##### 启动类

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

##### 方式一 ：启动类继承SpringBootServletInitializer实现configure

```
@SpringBootApplication
public class Application extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(Application.class);
    }
}
```

##### 方式二：新增加一个类继承SpringBootServletInitializer实现configure

```java
public class ServletInitializer extends SpringBootServletInitializer {
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        //此处的Application.class为带有@SpringBootApplication注解的启动类
        return builder.sources(Application.class);
    }
}
```



#### 使用mvn命令打包

```bash
mvn clean package -Dmaven.test.skip=true
```

