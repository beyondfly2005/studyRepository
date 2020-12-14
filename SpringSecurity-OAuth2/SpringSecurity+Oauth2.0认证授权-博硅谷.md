> 视频资源：https://www.bilibili.com/video/BV14z4y1d7eY
>
> 参考文档：https://blog.csdn.net/qq_38697437/article/details/106129153



## 6、OAuth2.0

### 6.1 OAuth2.0介绍

### 6.2 Spring Cloud Security OAuth2

#### 6.2.1 环境介绍

Spring-Security-OAuth2是对OAuth2的一种实现，并且跟我们之前学习的Spring-Security相辅相成

Spring-Security-OAuth2 包含两个服务：授权服务、资源服务

**授权服务（认证服务Author）**

**资源服务**

#### 6.2.2 环境搭建

##### 6.2.2.1 父工程

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifictId>spring-boot-starter-parent</artifictId>
    <version>2.1.3.RELEASE</version>
</parent>
<dependencyManagement>
    <dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Greenwich.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
    </dependency>
    <dependency>
    	<groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>javax.interceptor</groupId>
        <artifactId>javax.interceptor-api</artifactId>
        <version>1.2</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.47</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.0</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
    </dependency>
    
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-jwt</artifactId>
        <version>1.0.10.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.security.oauth.boot</groupId>
        <artifactId>spring-security-oauth2-autoconfigure</artifactId>
        <version>2.1.3.RELEASE</version>
    </dependency>
    </dependencies>
</dependencyManagement>
```

##### 6.2.2.2 创建UAA授权服务工程

1、创建distributed-security-uaa认证服务

```xml
netflix-hystrix
netflix-ribbon
stater-openfeign
<project>
    <dependency>
        <groupId>org.springframework.retry</groupId>
        <artifactId>spring-retry</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actutor</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-freemarker</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.data</groupId>
        <artifactId>spring-data-common</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-stater-security</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-stater-oauth2</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-cloud-stater-jwt</artifactId>
    </dependency>
</project>
```

2、启动类

```java
@SpringBootApplication
@EnableDiscoveryClient
@EnableHystrix
@EnableFeignClients(basePackages={"com.xx.security.distribute.uaa"})
public class UaaServer{
    public static  void main(String[] args){
        SpringApplication.run(UaaServer.class, args);
    }
}
```

3、配置文件 application.properties

```properties

```

##### 6.2.2.3 创建Order资源服务工程

1、导入依赖

```xml

```

2、启动类

```java


```

3、配置文件

```properties

```

#### 6.2.3 授权服务器配置

##### 6.2.3.1 EnableAuthorizationServer

可以用@EnableAuthorizationServer注解并继承AuthorizationServerConfigureAdapter来配置OAuth2.0授权服务器

在config包下创建AuthorizationServer

```java
@Configuration
@EnableAuthorizationServer
public class AuthorizationServer extends {
    AuthorizationSererConfigureAdapter{
        
    }
}
```

AuthorizationServerConfigureAdapter 要求配置一下几个类，这几个类是由Spring创建的独立的配置对象，它们会被`Spring`传入`AuthorizationServerConfigurer`中进行配置。

```java
public class AuthorizationServerConfigurerAdapter implements AuthorizationServerConfigurer { 
	public AuthorizationServerConfigurerAdapter(){}
	public void configure(AuthorizationServerSecurityConfigurer security){}
	public void configure(ClientDetailsServiceConfigurer clients) {}
	public void configure(AuthorizationServerEndpointsConfigurer endpoints){}
}
```

- **ClientDetailsServiceConfigurer**:用来配置客户端详情服务（`ClientDetailsService`），客户端详情信息在这里进行初始化，你能够把客户端详情信息写死在这里或者是通过数据库来存储调取详情信息
- **AuthorizationServerEndpointsConfigurer**:用来配置令牌（`token`）的访问端点和令牌服务(`token services`)
- **AuthorizationServerSecurityConfigurer**:用来配置令牌端点的安全约束

##### 6.2.3.2 配置客户端详细信息

```java
pubic class AuthorizationServer extends AuthorizationServer{
    //配置客户端详细信息服务
    @Overrride
    public void configure(ClientDetailsServiceConfigurer clients) throw Exception{
        cliets.inMeory()	//是由内存方式
            .whthClient("c1")	//client_id
            .secret(New BCryptPasswordEncoder().encode("secret"))
            .resourceIds("res1")
            .aotohorizedGrantTypes("authorization_code","password","client_credentials",)
			.scope("all") //允许的授权范围
            .autoApprove(false)
            .redirectUris("http://www.baidu.com");
    }
}
```

