Spring Security Oauth2.0

> 视频地址：https://www.bilibili.com/video/BV1Kb411u7VP
>
> 在线文档：https://www.funtl.com/zh/spring-security-oauth2/
>
> 作者博客：https://www.funtl.com

# Spring Security OAuth2.0

## 简介

Spring Security oAuth2-简介
### 概述
本章节的目的是帮助大家快速上手使用 Spring 提供的 Spring Security oAuth2 搭建一套验证授权及资源访问服务，帮助大家在实现企业微服务架构时能够有效的控制多个服务的统一登录、授权及资源保护工作

### 什么是 oAuth
oAuth 协议为用户资源的授权提供了一个安全的、开放而又简易的标准。与以往的授权方式不同之处是 oAuth 的授权不会使第三方触及到用户的帐号信息（如用户名与密码），即第三方无需使用用户的用户名与密码就可以申请获得该用户资源的授权，因此 oAuth 是安全的。

### 什么是 Spring Security
Spring Security 是一个安全框架，前身是 Acegi Security，能够为 Spring 企业应用系统提供声明式的安全访问控制。Spring Security 基于 Servlet 过滤器、IoC 和 AOP，为 Web 请求和方法调用提供身份确认和授权处理，避免了代码耦合，减少了大量重复代码工作。

## 为什么需要 oAuth2
### 本节视频
Spring Security oAuth2-为什么需要 oAuth2
Spring Security oAuth2-交互过程

### 应用场景
我们假设你有一个“云笔记”产品，并提供了“云笔记服务”和“云相册服务”，此时用户需要在不同的设备（PC、Android、iPhone、TV、Watch）上去访问这些“资源”（笔记，图片）

那么用户如何才能访问属于自己的那部分资源呢？此时传统的做法就是提供自己的账号和密码给我们的“云笔记”，登录成功后就可以获取资源了。但这样的做法会有以下几个问题：

“云笔记服务”和“云相册服务”会分别部署，难道我们要分别登录吗？
如果有第三方应用程序想要接入我们的“云笔记”，难道需要用户提供账号和密码给第三方应用程序，让他记录后再访问我们的资源吗？
用户如何限制第三方应用程序在我们“云笔记”的授权范围和使用期限？难道把所有资料都永久暴露给它吗？
如果用户修改了密码收回了权限，那么所有第三方应用程序会全部失效。
只要有一个接入的第三方应用程序遭到破解，那么用户的密码就会泄露，后果不堪设想。
为了解决如上问题，oAuth 应用而生。

### 名词解释
第三方应用程序（Third-party application）： 又称之为客户端（client），比如上节中提到的设备（PC、Android、iPhone、TV、Watch），我们会在这些设备中安装我们自己研发的 APP。又比如我们的产品想要使用 QQ、微信等第三方登录。对我们的产品来说，QQ、微信登录是第三方登录系统。我们又需要第三方登录系统的资源（头像、昵称等）。对于 QQ、微信等系统我们又是第三方应用程序。
HTTP 服务提供商（HTTP service）： 我们的云笔记产品以及 QQ、微信等都可以称之为“服务提供商”。
资源所有者（Resource Owner）： 又称之为用户（user）。
用户代理（User Agent）： 比如浏览器，代替用户去访问这些资源。
认证服务器（Authorization server）： 即服务提供商专门用来处理认证的服务器，简单点说就是登录功能（验证用户的账号密码是否正确以及分配相应的权限）
资源服务器（Resource server）： 即服务提供商存放用户生成的资源的服务器。它与认证服务器，可以是同一台服务器，也可以是不同的服务器。简单点说就是资源的访问入口，比如上节中提到的“云笔记服务”和“云相册服务”都可以称之为资源服务器。
### 交互过程
oAuth 在 "客户端" 与 "服务提供商" 之间，设置了一个授权层（authorization layer）。"客户端" 不能直接登录 "服务提供商"，只能登录授权层，以此将用户与客户端区分开来。"客户端" 登录授权层所用的令牌（token），与用户的密码不同。用户可以在登录的时候，指定授权层令牌的权限范围和有效期。"客户端" 登录授权层以后，"服务提供商" 根据令牌的权限范围和有效期，向 "客户端" 开放用户储存的资料。

![img](https://www.funtl.com/assets1/Lusifer_201904010001.png)

## 开放平台

### 交互模型
交互模型涉及三方：

资源拥有者：用户
客户端：APP
服务提供方：包含两个角色
认证服务器
资源服务器

### 认证服务器
认证服务器负责对用户进行认证，并授权给客户端权限。认证很容易实现（验证账号密码即可），问题在于如何授权。比如我们使用第三方登录 "有道云笔记"，你可以看到如使用 QQ 登录的授权页面上有 "有道云笔记将获得以下权限" 的字样以及权限信息



认证服务器需要知道请求授权的客户端的身份以及该客户端请求的权限。我们可以为每一个客户端预先分配一个 id，并给每个 id 对应一个名称以及权限信息。这些信息可以写在认证服务器上的配置文件里。然后，客户端每次打开授权页面的时候，把属于自己的 id 传过来，如：

http://www.funtl.com/login?client_id=yourClientId
随着时间的推移和业务的增长，会发现，修改配置的工作消耗了太多的人力。有没有办法把这个过程自动化起来，把人工从这些繁琐的操作中解放出来？当开始考虑这一步，开放平台的成型也就是水到渠成的事情了。

### oAuth2 开放平台
开放平台是由 oAuth2.0 协议衍生出来的一个产品。它的作用是让客户端自己去这上面进行注册、申请，通过之后系统自动分配 client_id ，并完成配置的自动更新（通常是写进数据库）。

客户端要完成申请，通常需要填写客户端程序的类型（Web、App 等）、企业介绍、执照、想要获取的权限等等信息。这些信息在得到服务提供方的人工审核通过后，开发平台就会自动分配一个 client_id 给客户端了。

到这里，已经实现了登录认证、授权页的信息展示。那么接下来，当用户成功进行授权之后，认证服务器需要把产生的 access_token 发送给客户端，方案如下：

让客户端在开放平台申请的时候，填写一个 URL，例如：http://www.funtl.com
每次当有用户授权成功之后，认证服务器将页面重定向到这个 URL（回调），并带上 access_token，例如：http://www.funtl.com?access_token=123456789
客户端接收到了这个 access_token，而且认证服务器的授权动作已经完成，刚好可以把程序的控制权转交回客户端，由客户端决定接下来向用户展示什么内容



# 创建工程案例



# 创建认证服务器

## 创建认证服务器模块

## 基于内存存储令牌

## 基于JDBC存储令牌

### 概述

本章节 **基于 JDBC 存储令牌** 的模式用于演示最基本的操作，帮助大家快速理解 oAuth2 认证服务器中 "认证"、"授权"、"访问令牌” 的基本概念

**操作流程**

![img](https://www.funtl.com/assets1/Lusifer_201904030001.png)

- 初始化 oAuth2 相关表

- 在数据库中配置客户端

- 配置认证服务器

  - 配置数据源：`DataSource`

  - 配置令牌存储方式：`TokenStore` -> `JdbcTokenStore`

  - 配置客户端读取方式：`ClientDetailsService` -> `JdbcClientDetailsService`

  - 配置服务端点信息：

    ```
    AuthorizationServerEndpointsConfigurer
    ```

    - `tokenStore`：设置令牌存储方式

  - 配置客户端信息：

    ```
    ClientDetailsServiceConfigurer
    ```

    - `withClientDetails`：设置客户端配置读取方式

- 配置 Web 安全

  - 配置密码加密方式：`BCryptPasswordEncoder`
  - 配置认证信息：`AuthenticationManagerBuilder`

- 通过 GET 请求访问认证服务器获取授权码

  - 端点：`/oauth/authorize`

- 通过 POST 请求利用授权码访问认证服务器获取令牌

  - 端点：`/oauth/token`

**附：默认的端点 URL**

- `/oauth/authorize`：授权端点
- `/oauth/token`：令牌端点
- `/oauth/confirm_access`：用户确认授权提交端点
- `/oauth/error`：授权服务错误信息端点
- `/oauth/check_token`：用于资源服务访问的令牌解析端点
- `/oauth/token_key`：提供公有密匙的端点，如果你使用 JWT 令牌的话

### 初始化OAuth2相关表

使用官方提供的建表脚本初始化 oAuth2 相关表，地址如下：

```text
https://github.com/spring-projects/spring-security-oauth/blob/master/spring-security-oauth2/src/test/resources/schema.sql
```



由于我们使用的是 MySQL 数据库，默认建表语句中主键为 `VARCHAR(256)`，这超过了最大的主键长度，请手动修改为 `128`，并用 `BLOB` 替换语句中的 `LONGVARBINARY` 类型，修改后的建表脚本如下：

```sql
CREATE TABLE `clientdetails` (
  `appId` varchar(128) NOT NULL,
  `resourceIds` varchar(256) DEFAULT NULL,
  `appSecret` varchar(256) DEFAULT NULL,
  `scope` varchar(256) DEFAULT NULL,
  `grantTypes` varchar(256) DEFAULT NULL,
  `redirectUrl` varchar(256) DEFAULT NULL,
  `authorities` varchar(256) DEFAULT NULL,
  `access_token_validity` int(11) DEFAULT NULL,
  `refresh_token_validity` int(11) DEFAULT NULL,
  `additionalInformation` varchar(4096) DEFAULT NULL,
  `autoApproveScopes` varchar(256) DEFAULT NULL,
  PRIMARY KEY (`appId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `oauth_access_token` (
  `token_id` varchar(256) DEFAULT NULL,
  `token` blob,
  `authentication_id` varchar(128) NOT NULL,
  `user_name` varchar(256) DEFAULT NULL,
  `client_id` varchar(256) DEFAULT NULL,
  `authentication` blob,
  `refresh_token` varchar(256) DEFAULT NULL,
  PRIMARY KEY (`authentication_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `oauth_approvals` (
  `userId` varchar(256) DEFAULT NULL,
  `clientId` varchar(256) DEFAULT NULL,
  `scope` varchar(256) DEFAULT NULL,
  `status` varchar(10) DEFAULT NULL,
  `expiresAt` timestamp NULL DEFAULT NULL,
  `lastModifiedAt` timestamp NULL DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `oauth_client_details` (
  `client_id` varchar(128) NOT NULL,
  `resource_ids` varchar(256) DEFAULT NULL,
  `client_secret` varchar(256) DEFAULT NULL,
  `scope` varchar(256) DEFAULT NULL,
  `authorized_grant_types` varchar(256) DEFAULT NULL,
  `web_server_redirect_uri` varchar(256) DEFAULT NULL,
  `authorities` varchar(256) DEFAULT NULL,
  `access_token_validity` int(11) DEFAULT NULL,
  `refresh_token_validity` int(11) DEFAULT NULL,
  `additional_information` varchar(4096) DEFAULT NULL,
  `autoapprove` varchar(256) DEFAULT NULL,
  PRIMARY KEY (`client_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `oauth_client_token` (
  `token_id` varchar(256) DEFAULT NULL,
  `token` blob,
  `authentication_id` varchar(128) NOT NULL,
  `user_name` varchar(256) DEFAULT NULL,
  `client_id` varchar(256) DEFAULT NULL,
  PRIMARY KEY (`authentication_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `oauth_code` (
  `code` varchar(256) DEFAULT NULL,
  `authentication` blob
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `oauth_refresh_token` (
  `token_id` varchar(256) DEFAULT NULL,
  `token` blob,
  `authentication` blob
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```



### 在数据库中配置客户端

在表 `oauth_client_details` 中增加一条客户端配置记录，需要设置的字段如下：

- `client_id`：客户端标识
- `client_secret`：客户端安全码，**此处不能是明文，需要加密**
- `scope`：客户端授权范围
- `authorized_grant_types`：客户端授权类型
- `web_server_redirect_uri`：服务器回调地址

使用 `BCryptPasswordEncoder` 为客户端安全码加密，代码如下：

```java
System.out.println(new BCryptPasswordEncoder().encode("secret"));
```

1

数据库配置客户端效果图如下：

![img](https://www.funtl.com/assets1/Lusifer_20190403150110.png)

### POM文件

由于使用了 JDBC 存储，我们需要增加相关依赖，数据库连接池部分弃用 `Druid` 改为 `HikariCP` （**号称全球最快连接池**）

```xml
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>${hikaricp.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
    <exclusions>
        <!-- 排除 tomcat-jdbc 以使用 HikariCP -->
        <exclusion>
            <groupId>org.apache.tomcat</groupId>
            <artifactId>tomcat-jdbc</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>${mysql.version}</version>
</dependency>
```



### 配置认证服务器

创建一个类继承 `AuthorizationServerConfigurerAdapter` 并添加相关注解：

- `@Configuration`
- `@EnableAuthorizationServer`

```java
package com.funtl.oauth2.server.config;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.security.oauth2.config.annotation.configurers.ClientDetailsServiceConfigurer;
import org.springframework.security.oauth2.config.annotation.web.configuration.AuthorizationServerConfigurerAdapter;
import org.springframework.security.oauth2.config.annotation.web.configuration.EnableAuthorizationServer;
import org.springframework.security.oauth2.config.annotation.web.configurers.AuthorizationServerEndpointsConfigurer;
import org.springframework.security.oauth2.provider.ClientDetailsService;
import org.springframework.security.oauth2.provider.client.JdbcClientDetailsService;
import org.springframework.security.oauth2.provider.token.TokenStore;
import org.springframework.security.oauth2.provider.token.store.JdbcTokenStore;

import javax.sql.DataSource;

@Configuration
@EnableAuthorizationServer
public class AuthorizationServerConfiguration extends AuthorizationServerConfigurerAdapter {

    @Bean
    @Primary
    @ConfigurationProperties(prefix = "spring.datasource")
    public DataSource dataSource() {
        // 配置数据源（注意，我使用的是 HikariCP 连接池），以上注解是指定数据源，否则会有冲突
        return DataSourceBuilder.create().build();
    }

    @Bean
    public TokenStore tokenStore() {
        // 基于 JDBC 实现，令牌保存到数据
        return new JdbcTokenStore(dataSource());
    }

    @Bean
    public ClientDetailsService jdbcClientDetails() {
        // 基于 JDBC 实现，需要事先在数据库配置客户端信息
        return new JdbcClientDetailsService(dataSource());
    }

    @Override
    public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
        // 设置令牌
        endpoints.tokenStore(tokenStore());
    }

    @Override
    public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
        // 读取客户端配置
        clients.withClientDetails(jdbcClientDetails());
    }
}
```

### 服务器安全配置

创建一个类继承 `WebSecurityConfigurerAdapter` 并添加相关注解：

- `@Configuration`
- `@EnableWebSecurity`
- `@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true, jsr250Enabled = true)`：全局方法拦截

```java
package com.funtl.oauth2.server.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true, jsr250Enabled = true)
public class WebSecurityConfiguration extends WebSecurityConfigurerAdapter {

    @Bean
    public BCryptPasswordEncoder passwordEncoder() {
        // 设置默认的加密方式
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {

        auth.inMemoryAuthentication()
                // 在内存中创建用户并为密码加密
                .withUser("user").password(passwordEncoder().encode("123456")).roles("USER")
                .and()
                .withUser("admin").password(passwordEncoder().encode("123456")).roles("ADMIN");

    }
}
```

### application.yml

```java
spring:
  application:
    name: oauth2-server
  datasource:
    type: com.zaxxer.hikari.HikariDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    jdbc-url: jdbc:mysql://192.168.141.128:3307/oauth2?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: 123456
    hikari:
      minimum-idle: 5
      idle-timeout: 600000
      maximum-pool-size: 10
      auto-commit: true
      pool-name: MyHikariCP
      max-lifetime: 1800000
      connection-timeout: 30000
      connection-test-query: SELECT 1

server:
  port: 8080
```

### 访问获取授权码

打开浏览器，输入地址：

```text
http://localhost:8080/oauth/authorize?client_id=client&response_type=code
```

第一次访问会跳转到登录页面

![img](https://www.funtl.com/assets1/Lusifer_20190401195014.png)

验证成功后会询问用户是否授权客户端

![img](https://www.funtl.com/assets1/Lusifer_20190401195129.png)

选择授权后会跳转到我的博客，浏览器地址上还会包含一个授权码（`code=1JuO6V`），浏览器地址栏会显示如下地址：

```text
http://www.funtl.com/?code=1JuO6V
```

有了这个授权码就可以获取访问令牌了

### 通过授权码向服务器申请令牌

通过 CURL 或是 Postman 请求

```bash
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'grant_type=authorization_code&code=1JuO6V' "http://client:secret@localhost:8080/oauth/token"
```

![img](https://www.funtl.com/assets1/Lusifer_20190402232952.png)

得到响应结果如下：

```json
{
    "access_token": "016d8d4a-dd6e-4493-b590-5f072923c413",
    "token_type": "bearer",
    "expires_in": 43199,
    "scope": "app"
}
```

操作成功后数据库 `oauth_access_token` 表中会增加一笔记录，效果图如下：

![img](https://www.funtl.com/assets1/Lusifer_20190403150529.png)