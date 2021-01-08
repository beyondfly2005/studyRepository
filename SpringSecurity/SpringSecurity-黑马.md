> Spring Security  https://www.bilibili.com/video/BV1fE411i7jt?seid=6782538689930778907



##### P1、 Spring Security介绍 

Spring Security 是一款强大的高度可定制的，认证、授权的框架

官网：https://spring.io/projects/spring-security

##### P2、案例介绍

半成品的后台管理系统 spring_security_managemen



##### P4、权限管理介绍

**2.1** **权限管理概念**

权限管理，一般指根据系统设置的安全规则或者安全策略，用户可以访问而且只能访问自己被授权的资源。权限管

理几乎出现在任何系统里面，前提是需要有用户和密码认证的系统。

> 在权限管理的概念中，有两个非常重要的名词：
>
> 认证：通过用户名和密码成功登陆系统后，让系统得到当前用户的角色身份。
>
> 授权：系统根据当前用户的角色，给其授予对应可以操作的权限资源

**2.2 完成权限管理需要三个对象**

用户：主要包含用户名，密码和当前用户的角色信息，可实现认证操作。

角色：主要包含角色名称，角色描述和当前角色拥有的权限信息，可实现授权操作。

权限：权限也可以称为菜单，主要包含当前权限名称，url地址等信息，可实现动态展示菜单。

> 注：这三个对象中，用户与角色是多对多的关系，角色与权限是多对多的关系，用户与权限没有直接关系，
>
> 二者是通过角色来建立关联关系的

P6、



**三、初识 Spring Security**

**3.1 Spring Security概念**

Spring Security是spring采用AOP思想，基于servlet过滤器实现的安全框架。它提供了完善的认证机制和方法级的

授权功能。是一款非常优秀的权限管理框架。

**3.2 Spring Security简单入门**

Spring Security博大精深，设计巧妙，功能繁杂，一言难尽，咱们还是直接上代码吧！

**3.2.1** **创建web工程并导入jar包**

##### P6、相关的Jar介绍

SprigSecurity是Spring采用AOP思想，基于servlet过滤器实现的安全框架，它提供了完整的认证机制和方法级的授权功能，是一款非常优秀的

pring Security主要jar包功能介绍

spring-security-core.jar

核心包，任何Spring Security功能都需要此包。

spring-security-web.jar

web工程必备，包含过滤器和相关的Web安全基础结构代码。

spring-security-confifig.jar

用于解析xml配置文件，用到Spring Security的xml配置文件的就要用到此包。

spring-security-taglibs.jar

Spring Security提供的动态标签库，jsp页面可以用。

spring-security-core.jar

spring-security-web

spring-secuirty-config.jar

sprign-security-taglibs.jar



使用maven

```xml
<dependency>
	<artifactId>org.springframework.security</artifactId>
    <groupId>spring-security-config</groupId>
    <version>5.1.5.RELEASE</version>
</dependency>
<dependency>
	<artifactId>org.springframework.security</artifactId>
    <groupId>spring-security-taglibs</groupId>
    <version>5.1.5.RELEASE</version>
</dependency>
```



3.2.2 配置web.xml

配置springsecurity核心过滤器链

```xml
<filter>
   <filter-name>springSecurityFilterChain></filter-name>
    <filter-clas>org.springframework.web.filter.DelegatingFilterProxy</filter-clas>
</filter>
<filter-mapping>

</filter-mapping>
```

spring-security.xml

```xml
<beans>

    <!--配置springsecurity-->
    <!--auto-concig="true" 表示自动加载springsecurity的配置文件 -->
    <!--use-expressions="true" 表示使用spring的el表达式 -->
    <security:http auto-concig="true" use-expressions="true">
        <security:intercept-url pattern="/**" access="hasAnyRole(‘ROLE_USER’)"
    </security:http>
</beans>
```





##### P33、SpringSecurity整合SpingBoot集中式版

技术选型：SpringBoot2.1.3 SpringSecurity MySQL5.7以上 Mybatis Jsp

初步认证第一版

创建工程并导入jar包

SpringBoot2.1.3 SpringSecurity

```xml

```



###### 导入SpringBoot的tomcat启动插件jar包

```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-jasper</artifactId>
        </dependency>
```



##### SpringSecurity整合SpingBoot分布式版

JWT+RSA

认证服务

资源服务