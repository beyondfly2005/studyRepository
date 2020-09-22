### SpringBoot 常见问题



##### SpringBoot如何集成Jsp



##### SpringBoot如何集成mybatis



##### SpringBoot @MapperScan扫描的是接口还是实现

扫描的是接口



##### SpringBoot Mapper文件不在resources目录下应如何配置

application.properties中增加

```properties
mybatis.mapper-locations=classpath*:com/ac/intellsecurity/dao/*/impl/*.xml
```

或者

application.yml中增加

```yaml
mybatis: 
  mapper-locations: classpath*:com/ac/intellsecurity/dao/*/impl/*.xml
```

还需要在pom.xml文件中增加  （目的：让idea识别，让maven识别）

```xml
<!-- 编译 src/main/java 目录下的 mapper 文件 -->
<resource>
    <directory>src/main/java</directory>
    <includes>
        <include>**/*.xml</include>
    </includes>
    <filtering>false</filtering>
</resource>
```

##### SpringBoot如何集成tk.mybatis



##### SpringBoot如何集成mybatis



##### MyBatis 报错无效的列类型

**解决方法**

**方法一：指定插入值得jdbcType，将sql改成 **

```sql
insert into user(id,name) values(#{id,jdbcType=VARCHAR},#{name,jdbcType=VARCHAR}) 
```

**方法二 在mybatis-config.xml文件中配置一下，添加settings配置，如下：(推荐)**

```html
<configuration>
<settings>
    <setting name="jdbcTypeForNull" value="NULL" />
</settings>
</configuration>
```

**springboot集成mybatis 修改 application.properties**

```properties
mybatis.configuration.jdbc-type-for-null=null
```

**springboot集成mybatis 修改application.yml**

```yaml
mybatis: 
  configuration:
    jdbc-type-for-null: null
```



springboot 集成 mybatis-plus时

```yaml
mybatis-plus:
  configuration:
    jdbc-type-for-null: ‘null‘
```



##### SpringBoot使用MyBatis报错：Error invoking SqlProvider method (tk.mybatis.mapper.provider.base.BaseInsertProvider.dynamicSQL)

> https://blog.csdn.net/qsx741olm/article/details/99010082

```java
//在SpringBoot的启动类上，使用@MapperScan注解时引入了错误的包下的。
//正确的应该是：
import tk.mybatis.spring.annotation.MapperScan;

//错误的引入了：
import org.mybatis.spring.annotation.MapperScan;
```



##### SpringBoot如何集成druid数据源



##### SpringBoot集成druid数据源时如何检查进行密码加密







##### springBoot 不同包下有同名类bean 启动报错问题

> 参考 https://blog.csdn.net/ZYC88888/article/details/84758835

```java
public class UniqueNameGenerator extends AnnotationBeanNameGenerator {
    @Override 
    public String generateBeanName(BeanDefinition definition, BeanDefinitionRegistry registry) { 
        //全限定类名 
        String beanName = definition.getBeanClassName(); 
        return beanName; 
    } 
}
```

在启动类上加注解@ComponentScan(nameGenerator = UniqueNameGenerator.class)使刚才我们自定义的BeanName生成策略生效。　

```java
@SpringBootApplication
@ComponentScan(nameGenerator = UniqueNameGenerator.class) 
public class BeanNameConflictApplication {
    public static void main(String[] args) {
        SpringApplication.run(BeanNameConflictApplication.class, args);
    } 
}
```



#####  java.lang.IllegalArgumentException: 在请求目标中找到无效字符。有效字符在RFC 7230和RFC 3986中定义



问题分析：

```
请求的URL地址中带有字符 ‘|’ ‘{’ ‘}’
在低版本的tomcat没有这个问题 tomcat 7.0.5xx 之前的版本
在7.0.5xx之后的版本 和8.xx之后的版本会有这个问题
```

###### 解决方案

```
在conf/catalina.properties文件中增加这样一条属性到最后：
tomcat.util.http.parser.HttpParser.requestTargetAllow=|{}
```

