##### 1、系统无法正常启动，报多个版本的的Sevlert-API ,在Servlet-AIP 2.4中 缺少 :java.lang.NoSuchMethodError: javax.servlet.ServletContext.*getVirtual*ServerName

###### 问题分析：

根据多个版本的Servlet-API判断 是由于多版本API冲突导致，个别jar包依赖旧版本的Servlet-API 2.4导致的

###### 解决方案：

使用Maven Heapler分析 Servlet-API 依赖冲突，找到低版本的Servlet-API 被哪个jar依赖，从依赖中排除；或者查看 新版本的jar 是否依赖较新版本的Servlet-API，尝试升级这个jar 以依赖新版本的servlert-API；

最终发现是net.sf.json-lib 依赖了servlet-api的旧版本，并1.02是最新版本 ，没有更新的版本，所有排除它对servelet-挨批2.4的依赖

```xml
        <dependency>
            <groupId>net.sf.json-lib</groupId>
            <artifactId>json-lib-ext-spring</artifactId>
            <version>1.0.2</version>
            <exclusions>
                <exclusion>
                    <artifactId>servlet-api</artifactId>
                    <groupId>javax.servlet</groupId>
                </exclusion>
            </exclusions>
        </dependency>
```



##### 1、Nginx中 session丢失问题

> https://www.jb51.net/article/187898.htm
>
> https://www.cnblogs.com/immer/p/8690180.html

```
location /health-dev/ {
  proxy_pass http://192.168.40.202:8080/health/;
  proxy_cookie_path /health /health-dev;
}
```



##### 自定义默认首页问题，默认是page/login.jsp 不是index.html ,无法识别默认首页，访问首页为空

> https://blog.csdn.net/flower_CSDN/article/details/81200682
>
> https://blog.csdn.net/u011429743/article/details/80117280
>
> https://www.iteye.com/blog/286-2413105

方法1：做地址访问控制

```java
@Controller
public class IndexController {

    @RequestMapping("/")
    public String index(){
        return "/page/login";
    }
}
```

方法2：添加配置文件

> https://blog.csdn.net/flower_CSDN/article/details/81200682

```java

@Configuration
public class MVCConfiguration extends WebMvcConfigurerAdapter {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) { 
        registry.addViewController("/").setViewName("forward:/login.html"); 
        registry.setOrder(Ordered.HIGHEST_PRECEDENCE); 
        super.addViewControllers(registry); 
    }
}
```

```java
@Configuration
public class DefaultView extends WebMvcConfigurerAdapter {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/Blog").setViewName("forward:index.jsp");
        registry.setOrder(Ordered.HIGHEST_PRECEDENCE);
        super.addViewControllers(registry);
    }
}
```



##### 2、项目中同时使用jsp和freeemarker，在访问首页时 发生了视图解析混乱问题

访问http://192.168.1.223:8081/IntellSecurity-web  做了访问控制 到 /page/login.jsp； 控制台输出 找不到/page/login.jsp.ftl 找不到 /page/login.jsp.html

这说明，在jsp返回的地址上，freemarker又做了一层视图解析

###### 解决方案：

> 参考 https://blog.csdn.net/a0b1c23/article/details/100020085
>
> https://www.cnblogs.com/cecWork/p/10515205.html

引入相关依赖

配置yml文件

```yml

#spring mvc 要修改默认目录，web-inf
spring:
  mvc:
    view:
      prefix: /WEB-INF/
  freemarker:
    template-loader-path: /WEB-INF/
```

修改启动类

```java

//需要继承SpringBootServletInitializer,当用wildfly容器时需要初始化servlet
@SpringBootApplication
public class BootApplication  extends SpringBootServletInitializer{
   @Override
   protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
      return application.sources(BootApplication.class);
   }
   public static void main(String[] args) {
      SpringApplication.run(BootApplication.class, args);
   }
}
```



##### 3、部分dao和mapper 无法绑定问题

org.apache.ibatis.binding.BindingException: Invalid bound statement (not found)错误

解决方案：

```properties
mybatis.mapper-locations=classpath*:com/ac/intellsecurity/dao/*/impl/*.xml
## 改为
mybatis.mapper-locations=classpath*:com/ac/intellsecurity/dao/**/impl/*.xml
```



##### 4、springboot Invalid character found in the request target 特殊字符传参报错

> https://www.jianshu.com/p/b6911b45bc02

```java
# 启动类中加入
@Bean
    public ConfigurableServletWebServerFactory webServerFactory() {
        TomcatServletWebServerFactory factory = new TomcatServletWebServerFactory();
        factory.addConnectorCustomizers((TomcatConnectorCustomizer) connector -> connector.setProperty("relaxedQueryChars", "|{}[]\\"));
        return factory;
    }
```

##### 5、通用报表 数据显示为 -  并不显示正确的数据





##### 6、返回前端的json时间格式不正确 返回的为Timestamp，应为格式化后的字符串如2020-08-08 12:00:00

问题分析：系统中采用了fastjson做的对象转json 返回给前端，应该是fastjson配置的问题

> springboot提供了两种配置方式，详见
>
> https://www.cnblogs.com/niunafei/p/11678921.html

问题处理：在FastJsonConfig 配置文件中增加对日期格式的处理

```
 fastJsonConfig.setSerializerFeatures(SerializerFeature.WriteDateUseDateFormat);
```

