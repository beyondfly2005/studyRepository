## Shiro权限管理框架

> 课程地址：https://www.bilibili.com/video/BV1j54y1t7jM?seid=1221486990855331641
>
> 源码地址：
>
> 

## 课程知识点

```
1、权限系统的整体概念
2、Shiro权限框架的核心组件
3、SpringBoot下Shiro的使用
4、shiro认证鉴权的缓存机制
5、分布式下使用shiro处理统一会话
6、密码重试次数，并发登录控制
7、前后端分离的鉴权方式
8、建立分布式统一鉴权系统
```

## 第一章 权限概述

### 1、什么是权限

权限管理，异步指根据系统设置的安全规则或者安全策略，用户可以访问而且只能访问自己被授权的资源，不多不少，权限管理几乎出现在任何系统里面，只要有用户和密码的系统。

权限管理一般在系统中一部分为：

- 访问权限

  ```
  一部表示你能做什么样的操作，或者能够访问哪些资源，例如张三赋予“店铺主管”角色，“店铺主管”具有“查询员工”、“添加员工”、“修改员工”和“删除员工”权限。此时张三能够进入系统，则可以进行这些操作。
  ```

  

- 数据权限

  ```java
  一般表示某些数据你是否属于你，或者属于你可以操作范围。例如：张三是"店铺主管"角色，他可以看他手下客服人员所有的服务的买家订单信息，他的手下只能看自己负责的订单信息。
  ```

### 2、认证的概念

#### 2.1 什么是认证

身份认证，就是判断一个用户是否为合法用户的处理过程。最常用的简单身份认证方式是系统通过核对用户输入的用户名和口令，看其是否与系统中存储的该用户的用户名和密码一致，来判断用户身份是否正确，例如：密码登录，手机短信验证，三方授权等。

#### 2.2 认证流程

用户名密码身份认证流程：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNjU5ODMwNy1lNjk1MjRkYzFiMmE3NDc4LnBuZw?x-oss-process=image/format,png)

#### 2.3 关键对象

上面的流程图中需要理解以下关键对象：

**Subject：**主体：访问系统的用户，主体可以是用户、程序等，进行认证的都称为主体。

**Principal：**身份信息，是主体(Subject)进行身份认证的标识，标识必须具有唯一性，如用户名、手机号、邮箱地址等，一个主体可以有多个身份，但是必须有一个主身份（Primary Principal）。

**credential：**凭证信息：是只有主体自己知道的安全信息，如密码、证书、手机短信验证码等。

### 3、授权的概念

#### 3.1 什么是授权

授权，即访问控制，控制谁可以访问哪些资源。主体进行身份认证后，系统会为其分配对应的权限，当访问资源时，会校验其是否具有访问此资源的权限。

这里首先理解4个对象：

- 用户对象user：当前操作的用户、程序
- 资源对象resource：当前被访问的对象
- 角色对象role：一组“权限操作许可权”的集合
- 权限对象permission：权限操作许可权

#### 3.2 授权流程

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNjU5ODMwNy1mYmIyNjVjODliNzYwMzMzLnBuZw?x-oss-process=image/format,png)

#### 3.3 关键对象

**授权可以简单理解为who对what进行了how操作**

**Who：主体(Subject)：**可以是一个用户，也可以是一个程序

**What：资源(Resource)：**如系统菜单、页面、按钮、方法、系统商品信息等。

**How：权限许可(Permission)**

​	我的商品（资源）===> 访问我的商品（权限许可）

​	分销商菜单（资源）===> 访问分销商列表（权限许可）



## 第二章 Shiro概述

### 1、Shiro简介

#### 1.1 什么是Shiro？

Shiro是apache旗下一个开源框架，它将软件系统的安全认证相关的功能抽取处理，实现用户身份认证、权限授权、加密、会话管理等功能，组成了一个通用的安全认证框架。

#### 1.2 Shiro的特点

Shiro是一个强大而灵活的开源安全框架，能够非常清晰的处理认证、授权、管理会话、及密码加密。如下是它所具有的特点：

- 易于理解的 Java Security API
- 简单的身份认证（登录），支持多种数据源（LDAP，JDBC等）
- 对角色的点点的鉴权（访问控制），也支持犀利的的鉴权
- 支持一级缓存，以提升应用程序的性能
- 内置的基于 POJO企业会话管理，适用于Web以及非Web的环境
- 异构客户端会话访问
- 非常简单的加密API
- 不跟任何的框架或者容器捆绑，可以独立运行

### 2、核心组件

- shiro架构图

  ![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNzk4NTYwMy1hMWVkNzkwNDM3YTg3NGYyLnBuZw?x-oss-process=image/format,png)

#### 2.1 Subject

   Subject即主体，外部应用与subject进行交互，subject记录了当前操作用户，将用户的概念理解为当前操作的主体，可能是一个通过浏览器请求的用户，也可能是一个运行的程序。     Subject在shiro中是一个接口，接口中定义了很多认证授相关的方法，外部程序通过subject进行认证授，而subject是通过SecurityManager安全管理器进行认证授权

#### 2.2 SecurityManager

  ​    SecurityManager即安全管理器，对全部的subject进行安全管理，它是shiro的核心，负责对所有的subject进行安全管理。通过SecurityManager可以完成subject的认证、授权等，实质上SecurityManager是通过Authenticator进行认证，通过Authorizer进行授权，通过SessionManager进行会话管理等。

  ​     SecurityManager是一个接口，继承了Authenticator,Authorizer, SessionManager这三个接口。

#### 2.3 Authenticator

  ​    Authenticator即认证器，对用户身份进行认证，Authenticator是一个接口，shiro提供ModularRealmAuthenticator实现类，通过ModularRealmAuthenticator基本上可以满足大多数需求，也可以自定义认证器。

#### 2.4 Authorizer

  ​     Autorizer即授权器，用户通过认证器认证通过，在访问功能时需要通过授权器判断用户是否有此功能的操作权限。   

#### 2.5 Realm

  ​     Realm即领域，相当于datasource数据源，securityManager进行安全认证需要通过Realm获取用户权限数据，比如：如果用户身份数据在数据库那么realm就需要从数据库获取用户身份信息。

  ​     注意：不要把realm理解成只是从数据源取数据，在realm中还有认证授权校验的相关的代码。2.6

####  2.6 SessionManager

SessionManager会话管理，shiro框架定义了一套会话管理，他不依赖ewb容器的session，所以shiro可以使用在非web应用上，也可以将分布式英勇的会话集中在一点进行管理，磁特性可使他实现单点登录。

#### 2.7 SessionDao

SessionDao即会话dao，是对session会话操作的一套接口，比如：

​	可以通过Jdbc将会话存储到数据库

​	也可以吧session存储到缓存服务器

#### 2.8 CacheManager

CacheManager缓存管理，将用户权限数据存储在缓存，这样可以提高性能

#### 2.9 Cryptography

Cryptography密码管理，shiro提供了一套加密/解密组件，方便开发，比如提供常用的三界、加解密等功能



## 第三章 Shiro入门

### 1、身份认证

#### 1.1 基本流程

![img](http://www.itheima.com/images/newslistPIC/1596520320959_Shiro%E5%85%A5%E9%97%A8%EF%BC%9A%E8%BA%AB%E4%BB%BD%E8%AE%A4%E8%AF%81%E5%B0%81%E9%9D%A2.jpg)

流程如下：

1、Shiro把用户的数据封装成标识token，token一般封装着用户名，密码等信息；

2、使用Subject门面获取到封装着用户的数据的标识token；

3、Subject把标识token交给SecurityManager，在SecurityManager安全中心中，SecurityManager把标识token委托给认证器；Authenticator进行身份验证。认证器的作用一般是用来指定如何验证，它规定本次认证用到哪些Realm；

4、认证器Authenticator将传入的标识token，与数据源Realm对比，验证token是否合法。 

#### 1.2 案例演示

##### 1.2.1 需求

使用shiro完成一个用户的登录

##### 1.2.2 实现

###### 1.2.2.1 新建项目

shiro-day01-01authenticator

![1596520194957_Shiro入门：身份认证02.jpg](http://www.itheima.com/images/newslistPIC/1596520194957_Shiro%E5%85%A5%E9%97%A8%EF%BC%9A%E8%BA%AB%E4%BB%BD%E8%AE%A4%E8%AF%8102.jpg)

###### 1.2.2.2 导入依赖

```xml
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.1.3</version>
        </dependency>
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-core</artifactId>
            <version>1.3.2</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- compiler插件, 设定JDK版本 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>8</source>
                    <target>8</target>
                    <showWarnings>true</showWarnings>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

###### 1.2.2.3 编写shiro.ini

```ini
#声明用户账号
[users]
jay=123
```

###### 1.2.2.4 编写HelloShiro

```java
package com.beyondsoft.shiro;

import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.config.IniSecurityManagerFactory;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.Factory;
import org.junit.Test;

/**
 * @Description：shiro的第一个例子
 */
public class HelloShiro {

    @Test
    public void shiroLogin() {
        //导入权限ini文件构建权限工厂
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        //工厂构建安全管理器
        SecurityManager securityManager = factory.getInstance();
        //使用SecurityUtils工具生效安全管理器
        SecurityUtils.setSecurityManager(securityManager);
        //使用SecurityUtils工具获得主体
        Subject subject = SecurityUtils.getSubject();
        //构建账号token
        UsernamePasswordToken usernamePasswordToken = new UsernamePasswordToken("jay", "123");
        //登录操作
        subject.login(usernamePasswordToken);
        System.out.println("是否登录成功：" + subject.isAuthenticated());
    }
}
```



###### 1.2.2.5 测试

![1596520223591_Shiro入门：身份认证03.jpg](http://www.itheima.com/images/newslistPIC/1596520223591_Shiro%E5%85%A5%E9%97%A8%EF%BC%9A%E8%BA%AB%E4%BB%BD%E8%AE%A4%E8%AF%8103.jpg)



##### 1.2.3 小结

(1) 权限定义：ini文件

(2) 加载过程：

导入权限ini文件构建权限工厂;

工厂构建安全管理器;

使用SecurityUtils工具生效安全管理器;

使用SecurityUtils工具获得主体;

使构建账号token用SecurityUtils工具获得主体;

构建账号token;

登录操作。

### 2、Realm

为了快速上手，我们需要知道的是，Shiro将数据库中的数据，存放到Realm这种对象中。而Shiro提供的Realm体系较为复杂，一般我们为了使用Shiro的基本目的就是：认证、授权。

#### 2.1 Realm接口

![img](https://img-blog.csdnimg.cn/20181218224546324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lhbmx1YW5kYWkxOTg1,size_16,color_FFFFFF,t_70)

所以，一般在真实的项目中，我们不会直接实现Realm接口，我们一般的情况就是直接继承AuthorizingRealm，能够继承到认证与授权功能。它需要强制重写两个方法：

```java
public class DefaultRealm extends AuthorizingRealm {
  
    //强制重写的认证方法
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        return null;
    }
    //强制重新授权方法，以后再说
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }
}
```

#### 2.2 自定义Realm

##### 2.2.1 需求

```
自定义Realm，取得密码用于比较
```

##### 2.2.2 实现

###### 2.2.2.1 创建项目

shiro-day01-02realm

![img](http://www.itheima.com/images/newslistPIC/1597386505185_Realm%E6%8E%A5%E5%8F%A302.jpg)

###### 2.2.2.2 定义SecurityService

SecurityService

```java
package com.itheima.shiro.service;

/**
 * @Description：权限服务接口
 */
public interface SecurityService {

  /**
    * @Description 查找密码按用户登录名
    * @param loginName 登录名称
    * @return
   */
  String findPasswordByLoginName(String loginName);
}
```

SecurityServiceImpl

```java
package com.itheima.shiro.service.impl;

import com.itheima.shiro.service.SecurityService;

/**
 * @Description：权限服务层
 */
public class SecurityServiceImpl implements SecurityService {

  @Override
  public String findPasswordByLoginName(String loginName) {
      return "123";
  }
}
```

###### 2.2.2.3 定义DefinitionRealm

```java
package com.itheima.shiro.realm;

import com.itheima.shiro.service.SecurityService;
import com.itheima.shiro.service.impl.SecurityServiceImpl;
import org.apache.shiro.authc.*;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;

/**
 * @Description：声明自定义realm
 */
public class DefinitionRealm extends AuthorizingRealm {

    /**
     * @Description 认证接口
     * @param token 传递登录token
     * @return
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        //从AuthenticationToken中获得登录名称
        String loginName = (String) token.getPrincipal();
        SecurityService securityService = new SecurityServiceImpl();
        String password = securityService.findPasswordByLoginName(loginName);
        if ("".equals(password)||password==null){
            throw new UnknownAccountException("账户不存在");
        }
        //传递账号和密码
        return  new SimpleAuthenticationInfo(loginName,password,getName());
    }

    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        return null;
    }
}
```

###### 2.2.2.4 编写shiro.ini

```ini
#声明自定义的realm,且为安全管理器指定realms
[main]
definitionRealm=com.beyondsoft.shiro.realm.DefinitionRealm
securityManager.realms=$definitionRealm
```

#### 2.3 认证源码跟踪

(1) 通过debug模式追踪源码subject.login(token) 发现。首先是进入Subject接口的默认实现类。果然，Subject将用户的用户名密码委托给了securityManager去做。

![1597386632831_Realm接口03.jpg](http://www.itheima.com/images/newslistPIC/1597386632831_Realm%E6%8E%A5%E5%8F%A303.jpg)


(2) 然后，securityManager说：“卧槽，认证器authenticator小弟，听说你的大学学的专业就是认证呀，那么这个认证的任务就交给你咯”。遂将用户的token委托给内部认证组件authenticator去做。

![1597386641661_Realm接口04.jpg](http://www.itheima.com/images/newslistPIC/1597386641660_Realm%E6%8E%A5%E5%8F%A304.jpg)


(3) 事实上，securityManager的内部组件一个比一个懒。内部认证组件authenticator说：“你们传过来的token我需要拿去跟数据源Realm做对比，这样吧，这个光荣的任务就交给Realm你去做吧”。Realm对象：“一群大懒虫!”。

![1597386651080_Realm接口05.jpg](http://www.itheima.com/images/newslistPIC/1597386651080_Realm%E6%8E%A5%E5%8F%A305.jpg)


(4) Realm在接到内部认证组件authenticator组件后很伤心，最后对电脑前的你说：“大兄弟，对不住了，你去实现一下呗”。从图中的方法体中可以看到，当前对象是Realm类对象，即将调用的方法是doGetAuthenticationInfo(token)。而这个方法，就是你即将要重写的方法。如果帐号密码通过了，那么返回一个认证成功的info凭证。如果认证失败，抛出一个异常就好了。你说：“什么?最终还是劳资来认证?”没错，就是苦逼的你去实现了，谁叫你是程序猿呢。所以，你不得不查询一下数据库，重写doGetAuthenticationInfo方法，查出来正确的帐号密码，返回一个正确的凭证info。

![1597386660293_Realm接口06.jpg](http://www.itheima.com/images/newslistPIC/1597386660293_Realm%E6%8E%A5%E5%8F%A306.jpg)


(5)好了，这个时候你自己编写了一个类，继承了AuthorizingRealm，并实现了上述doGetAuthenticationInfo方法。你在doGetAuthenticationInfo中编写了查询数据库的代码，并将数据库中存放的用户名与密码封装成了一个AuthenticationInfo对象返回。可以看到下图中，info这个对象是有值的，说明从数据库中查询出来了正确的帐号密码。

![1597386670069_Realm接口07.jpg](http://www.itheima.com/images/newslistPIC/1597386670069_Realm%E6%8E%A5%E5%8F%A307.jpg)


(6)那么，接下来就很简单了。把用户输入的帐号密码与刚才你从数据库中查出来的帐号密码对比一下即可。token封装着用户的帐号密码，AuthenticationInfo封装着从数据库中查询出来的帐号密码。再往下追踪一下代码，最终到了下图中的核心区域。如果没有报异常，说明本次登录成功。

![1597386678516_Realm接口08.jpg](http://www.itheima.com/images/newslistPIC/1597386678516_Realm%E6%8E%A5%E5%8F%A308.jpg)

### 3、编码、散列算法

#### 3.1 编码与解码

Shiro提供了base64和16进制字符串编码/解码的API支持，方便一些编码解码操作。

Shiro内部的一些数据的【存储/表示】都使用了base64和16进制字符串

##### 3.1.1 需求

```
理解base64和16进制字符串编码/解码
```

##### 3.1.2 新建项目

新建shiro-03encode-decode

![img](http://www.itheima.com/images/newslistPIC/1597387876261_%E7%BC%96%E7%A0%81%E6%95%A3%E5%88%97%E7%AE%97%E6%B3%9501.jpg)

##### 3.1.3 新建EncodeUtil

```java
package com.beyondsoft.shiro.util;

import org.apache.shiro.codec.Base64;
import org.apache.shiro.codec.Hex;

/**
 * @Description：封装base64和16进制编码解码工具类
 */
public class EncodesUtil {

    /**
     * @Description HEX-byte[]--String转换
     * @param input 输入数组
     * @return String
     */
    public static String encodeHex(byte[] input){
        return Hex.encodeToString(input);
    }

    /**
     * @Description HEX-String--byte[]转换
     * @param input 输入字符串
     * @return byte数组
     */
    public static byte[] decodeHex(String input){
        return Hex.decode(input);
    }

    /**
     * @Description Base64-byte[]--String转换
     * @param input 输入数组
     * @return String
     */
    public static String encodeBase64(byte[] input){
        return Base64.encodeToString(input);
    }

    /**
     * @Description Base64-String--byte[]转换
     * @param input 输入字符串
     * @return byte数组
     */
    public static byte[] decodeBase64(String input){
        return Base64.decode(input);
    }
}
```

##### 3.1.4 新建ClientTest

```java
package com.beyondsoft.shiro.client;

import com.beyondsoft.shiro.util.EncodesUtil;
import org.junit.Test;

/**
 * 测试编码、解码
 */
public class ClientTest {

    @Test
    public void testHex(){
        String val = "hello";
        String flag = EncodesUtil.encodeHex(val.getBytes());
        String valHandler = new String(EncodesUtil.decodeHex(flag));
        System.out.println("比较结果："+val.equals(valHandler));
    }

    @Test
    public void testBase64(){
        String val = "hello";
        String flag = EncodesUtil.encodeBase64(val.getBytes());
        String valHandler = new String(EncodesUtil.decodeBase64(flag));
        System.out.println("比较结果："+val.equals(valHandler));
    }
}
```

##### 3.1.5 小结

1、shiro目前支持的编码与解码：

·base64

·(HEX)16进制字符串

2、那么shiro的编码与解码什么时候使用呢?又是怎么使用的呢?

#### 3.2 散列算法

散列算法一般用于生成数据的摘要信息，是一种不可逆的算法，一般适合存储密码之类的数据，常见的散列算法如MD5、SHA等。一般进行散列时最好提供一个salt(盐)，比如加密密码“admin”，产生的散列值是“21232f297a57a5a743894a0e4a801fc3”，可以到一些md5解密网站很容易的通过散列值得到密码“admin”，即如果直接对密码进行散列相对来说破解更容易，此时我们可以加一些只有系统知道的干扰数据，如salt(即盐);这样散列的对象是“密码+salt”，这样生成的散列值相对来说更难破解。

**shiro支持的散列算法：**

**Md2Hash、Md5Hash、Sha1Hash、Sha256Hash、Sha384Hash、Sha512Hash**

![img](http://www.itheima.com/images/newslistPIC/1597387887175_%E7%BC%96%E7%A0%81%E6%95%A3%E5%88%97%E7%AE%97%E6%B3%9502.jpg)

##### 3.2.1 新增DigestsUtil

```java
package com.beyondsoft.shiro.util;

import org.apache.shiro.crypto.SecureRandomNumberGenerator;
import org.apache.shiro.crypto.hash.SimpleHash;

import java.util.HashMap;
import java.util.Map;

/**
 * 生成摘要
 */
public class DigestsUtil {

    private static final String SHA1 = "SHA-1";

    private static final Integer ITERATIONS = 512;

    /**
     * @Description sha1摘要算法
     * @param input 需要散列字符串
     * @param salt 盐字符串 干扰数据
     * @return
     */
    public static String sha1(String input, String salt){
        return new SimpleHash(SHA1, input, salt,ITERATIONS).toString();
    }

    /**
     * @Description 随机生成salt字符串
     * @return hex编码的slat
     */
    public static String generateSalt(){
        SecureRandomNumberGenerator randomNumberGenerator = new SecureRandomNumberGenerator();
        return randomNumberGenerator.nextBytes().toHex();
    }

    /**
     * @Description 生成密码字符密文和salt密文
     * @param
     * @return
     */
    public static Map<String,String> entryptPassword(String passwordPlain) {
        Map<String,String> map = new HashMap<String,String>();
        String salt = generateSalt();
        String password = sha1(passwordPlain, salt);
        map.put("salt", salt);
        map.put("password", password);
        return map;
    }
}
```

### 4、Realm使用散列算法

在realm中怎么使用散列算法？在shiro-day01-02realm中我们使用的密码是明文的校验方式，也就是SecurityServiceImpl中findPasswordByLoginName返回的是明文123的密码。

```java
package com.itheima.shiro.service.impl;
import com.itheima.shiro.service.SecurityService;
/**
 * @Description：权限服务层
 */
public class SecurityServiceImpl implements SecurityService {
    @Override
    public String findPasswordByLoginName(String loginName) {
        return "123";
    }
}
```



#### 4.1 新建项目

shiro-05ciphertext-realm

![img](http://www.itheima.com/images/newslistPIC/1597388982126_%E7%BC%96%E7%A0%81%E6%95%A3%E5%88%97%E7%AE%97%E6%B3%953.jpg)

#### 4.2 创建密文密码

使用ClientTest的testDigestsUtil创建密码为“123”的password密文和salt密文。

```
password:56265d624e484ca62c6dfbc523e6d6fc7932d0d5
salt:845a66ac80174c0e486db9354cf84f9a
```

#### 4.3 修改SecurityService

SecurityService修改成返回salt和password的map

```java
package com.itheima.shiro.service;

import java.util.Map;

/**
 * @Description：权限服务接口
 */
public interface SecurityService {

    /**
     * @Description 查找密码按用户登录名
     * @param loginName 登录名称
     * @return
     */
    Map<String,String> findPasswordByLoginName(String loginName);
}
package com.itheima.shiro.service.impl;

import com.itheima.shiro.service.SecurityService;

import java.util.HashMap;
import java.util.Map;

/**
 * @Description：权限服务层
 */
public class SecurityServiceImpl implements SecurityService {

    @Override
    public Map<String,String> findPasswordByLoginName(String loginName) {
        //模拟数据库中存储的密文信息
       return  DigestsUtil.entryptPassword("123");
    }
}
```



#### 4.4 指定密码匹配方式

为DefinitionRealm类添加构造方法如下：

```java
    /**
     * @Description 构造函数
     */
    public DefinitionRealm() {
        //指定密码匹配方式为sha1
        HashedCredentialsMatcher matcher = new HashedCredentialsMatcher(DigestsUtil.SHA1);
        //指定密码迭代次数
        matcher.setHashIterations(DigestsUtil.ITERATIONS);
        //使用父亲方法使匹配方式生效
        setCredentialsMatcher(matcher);
    }
```

修改DefinitionRealm类的认证doGetAuthenticationInfo方法如下

```java
/**
     * @Description 认证接口
     * @param token 传递登录token
     * @return
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        //从AuthenticationToken中获得登录名称
        String loginName = (String) token.getPrincipal();
        SecurityService securityService = new SecurityServiceImpl();
        Map<String, String> map = securityService.findPasswordByLoginName(loginName);
        if (map.isEmpty()){
            throw new UnknownAccountException("账户不存在");
        }
        String salt = map.get("salt");
        String password = map.get("password");
        //传递账号和密码:参数1：缓存对象，参数2：明文密码，参数三：字节salt,参数4：当前DefinitionRealm名称
        return  new SimpleAuthenticationInfo(loginName,password, ByteSource.Util.bytes(salt),getName());
    }
```



#### 4.5 测试

测试

![1597388994262_编码散列算法03.jpg](http://www.itheima.com/images/newslistPIC/1597388994262_%E7%BC%96%E7%A0%81%E6%95%A3%E5%88%97%E7%AE%97%E6%B3%9503.jpg)

![1597389002801_编码散列算法04.jpg](http://www.itheima.com/images/newslistPIC/1597389002801_%E7%BC%96%E7%A0%81%E6%95%A3%E5%88%97%E7%AE%97%E6%B3%9504.jpg)

![1597389012002_编码散列算法05.jpg](http://www.itheima.com/images/newslistPIC/1597389012002_%E7%BC%96%E7%A0%81%E6%95%A3%E5%88%97%E7%AE%97%E6%B3%9505.jpg)

![1597389020403_编码散列算法06.jpg](http://www.itheima.com/images/newslistPIC/1597389020403_%E7%BC%96%E7%A0%81%E6%95%A3%E5%88%97%E7%AE%97%E6%B3%9506.jpg)

### 5、身份授权

#### 5.1 基本流程

1、首先调用Subject.isPermitted/hasRole接口，其会委托给SecurityManager

2、SecurityManager接着会委托给内部组件Authorizer；

3、Authorizer再将其请求委托给我们的Realm去做，Realm才是真正的干活的；

4、Realm将用户请求的参数封装成权限对象。再从我们重写的doGetAuthorizationInfo方法中获取从数据库中查询到的权限集合。

5、Realm将用户传入的权限对象，与从数据库中查出来的权限对象，进行一一对比。如果用户传入的权限对象在从数据库中查出来的权限对象中，则返回true，否则返回false。

进行授权操作的前提：用户必须通过认证。

在真实的项目中，角色与权限都存放在数据库中，为了快速上手 ，我们先创建一个自定义DefinitionRealm，模拟它已经等了成功。直接返回一个等了验证凭证，告诉Shiro框架，我们从数据库中查询出来的密码也就是你输入的密码。所以，不管用户输入什么，本次等了验证都是通过的。

```java
    /**
     * 认证方法
     * @param authenticationToken
     * @return
     * @throws AuthenticationException
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        //获取登录名
        SecurityService securityService = new SecurityServiceImpl();
        String loginName = (String)authenticationToken.getPrincipal();
        Map<String, String> map = securityService.findPassByLoginName(loginName);
        if(map.isEmpty()) {
            throw new UnknownAccountException("账号不存在！");
        }
        String salt = map.get("salt");
        String password = map.get("password");
        return new SimpleAuthenticationInfo(loginName, password, ByteSource.Util.bytes(salt), getName());
    }
```

好了，接下来，我们要重新我们本小结的核心方法了，在DefinitionRealm中找到下列方法：

```java
@Override
protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principal) {
    return null;
}
```

此方法的传入参数PrincipalCollection principal，是一个包装对象，它表示“用户认证凭证信息”。包装的是谁呢？没错，就是认证doGetAuthenticationInfo（）方法的返回值的第一个参数loginname。你可以通过这个包装对象的getPrimaryPrincipal())方法拿到此值，然后再从数据库中拿到对应的角色和资源，构建SimpleAuthorizationInfo

```java
    //授权
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        String loginName = (String) principals.getPrimaryPrincipal();
		//从数据库中查询对应的角色和资源
        SecurityService securityService = new  SecurityServiceImpl();
        List<String> roles = securityService.findRoleByLoginName(loginName);
        List<String> permissions = securityService.findPermissionByLoginName(loginName);
        //构造一个授权凭证
        SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
        //通过你的用户名查询数据库，得到你的权限信息与角色信息。并存放到权限凭证中
        info.addRoles(roles);
        info.addStringPermissions(permissions);
        //返回你的权限信息
        return info;
    }

```

#### 5.2 案例演示

##### 5.2.1 需求

```
1、实现doGetAuthorizationInfo方法实现鉴权
2、使用Subject类实现权限的校验
```

##### 5.2.2 实现

###### 5.2.2.1 创建项目

新建shiro-06-authentication-realm

###### 5.2.2.2 编写SecurityService

SecurityService

```java
package com.beyondsoft.shiro.service;

import java.util.List;
import java.util.Map;

/**
 * 模拟数据库操作
 */
public interface SecurityService {

    /**
     * 查找用户名 密码
     * @param loginName 用户名称
     * @return 密码
     */
    Map<String,String> findPassByLoginName(String loginName);

    /**
     * 查询角色
     * @param loginName 用户名
     * @return 角色列表
     */
    List<String> findRoleByLoginName(String loginName);

    /**
     * 查询资源
     * @param loginName 用户名
     * @return 资源字符串列表
     */
    List<String> findPermissionByLoginName(String loginName);
}
```

SecurityServiceImpl

```java
package com.beyondsoft.shiro.service;

import com.beyondsoft.shiro.util.DigestsUtil;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;

/**
 * 模拟数据库服务接口的实现
 */
public class SecurityServiceImpl implements SecurityService {

    @Override
    public Map<String,String> findPassByLoginName(String loginName) {
        return DigestsUtil.entryptPassword("123") ;
    }

    @Override
    public List<String> findRoleByLoginName(String loginName) {
        List<String> roles = new ArrayList<>();
        roles.add("admin");
        roles.add("dev");
        return roles;
    }

    @Override
    public List<String> findPermissionByLoginName(String loginName) {
        List<String> list = new ArrayList<>();
        list.add("order:add");
        list.add("order:list");
        list.add("order:delete");
        return list;
    }
}
```

###### 5.2.2.3 编写DefinitionRealm 

```java
package com.beyondsoft.shiro.realm;

import com.beyondsoft.shiro.service.SecurityService;
import com.beyondsoft.shiro.service.SecurityServiceImpl;
import com.beyondsoft.shiro.util.DigestsUtil;
import org.apache.shiro.authc.*;
import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.util.ByteSource;

import java.util.List;
import java.util.Map;

public class DefinitionRealm extends AuthorizingRealm {

    public DefinitionRealm(){
        //指定密码匹配方式sha1
        HashedCredentialsMatcher hashedCredentialsMatcher = new HashedCredentialsMatcher(DigestsUtil.SHA1);
        //指定密码迭代次数
        hashedCredentialsMatcher.setHashIterations(DigestsUtil.ITERATIONS);
        //使用父层方法使得匹配方式生效
        setCredentialsMatcher(hashedCredentialsMatcher);
    }

    /**
     * 认证方法
     * @param authenticationToken
     * @return
     * @throws AuthenticationException
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        //获取登录名
        SecurityService securityService = new SecurityServiceImpl();
        String loginName = (String)authenticationToken.getPrincipal();
        Map<String, String> map = securityService.findPassByLoginName(loginName);
        if(map.isEmpty()) {
            throw new UnknownAccountException("账号不存在！");
        }
        String salt = map.get("salt");
        String password = map.get("password");
        return new SimpleAuthenticationInfo(loginName, password, ByteSource.Util.bytes(salt), getName());
    }

    /**
     * 鉴权方法
     * @param principalCollection
     * @return
     */
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        // 拿到用户凭证信息
        String loginName = (String) principalCollection.getPrimaryPrincipal();
        // 从数据库中查询对应的角色和权限
        SecurityService securityService = new  SecurityServiceImpl();
        List<String> roles = securityService.findRoleByLoginName(loginName);
        List<String> permissions = securityService.findPermissionByLoginName(loginName);
        // 构建资源交易对象
        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo();
        simpleAuthorizationInfo.addRoles(roles);
        simpleAuthorizationInfo.addStringPermissions(permissions);
        //返回你的权限信息
        return simpleAuthorizationInfo;
    }
}
```

###### 5.2.2.3 编写HelloShiro

```java
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.config.IniSecurityManagerFactory;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.Factory;
import org.junit.Test;

/**
 * @Description：shiro的第一个例子
 */
public class HelloShiro {

    @Test
    public void testPermissionRealm(){
        Subject subject = shiroLogin();
        System.out.println("登录结果："+subject.isAuthenticated());
        System.out.println("是否拥有管理员角色："+subject.hasRole("admin"));
        try {
            subject.checkRole("coder");
            System.out.println("当前用户拥有coder角色");
        } catch (Exception e){
            System.out.println("当前用户没有coder角色");
        }
        System.out.println("是否拥有查看订单的权限"+subject.isPermitted("order:list"));
        try{
            subject.checkPermission("order:update");
            System.out.println("当前用户拥有订单修改权限");
        } catch (Exception e) {
            System.out.println("当前用户拥有订单修改权限");
        }
    }

    public Subject shiroLogin() {
        //导入权限ini文件构建权限工厂
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        //工厂构建安全管理器
        SecurityManager securityManager = factory.getInstance();
        //使用SecurityUtils工具生效安全管理器
        SecurityUtils.setSecurityManager(securityManager);
        //使用SecurityUtils工具获得主体
        Subject subject = SecurityUtils.getSubject();
        //构建账号token
        UsernamePasswordToken usernamePasswordToken = new UsernamePasswordToken("jay", "123");
        //登录操作
        subject.login(usernamePasswordToken);
        System.out.println("是否登录成功：" + subject.isAuthenticated());
        return subject;
    }
}
```



##### 5.2.3 授权码跟踪



#### 5.3 小结



##  第四章 Web项目集成Shiro

### 1、Web集成原理分析

#### 1.1 web集成的配置

还记得吗，以前我们在没有与WEB环境进行集成的时候，为了生成SecurityManager对象，是通过手动读取配置文件生存工厂对象，再通过工厂对象获取到SecurityManager的。就像下面代码展示的那样

```java
/**
 * @Description：登录方法
 */
public class HelloShiro {

    @Test
    public Subject shiroLogin() {
        //导入权限ini文件构建权限工厂
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        //工厂构建安全管理器
        SecurityManager securityManager = factory.getInstance();
        //使用SecurityUtils工具生效安全管理器
        SecurityUtils.setSecurityManager(securityManager);
        //使用SecurityUtils工具获得主体
        Subject subject = SecurityUtils.getSubject();
        //构建账号token
        UsernamePasswordToken usernamePasswordToken = new UsernamePasswordToken("jay", "123");
        //登录操作
        subject.login(usernamePasswordToken);
        return subject;        
    }
}
```

不过，现在我们既然说要与WEB集成，那么首先要做的事情就是把我们的shiro.ini这个配置文件交付到WEB环境中。定义shiro.ini文件如下

```ini
#声明自定义的realm,且为安全管理器指定realms
[main]
definitionRealm=com.beyondsoft.shiro.realm.DefinitionRealm
securityManager.realms=$definitionRealm
```

##### 1.1.1 新建项目

新建web项目shiro-07web，其中realm、service、resource内容从shiro-06authentication-realm中拷贝即可

##### 1.1.2 pom.xml

```xml
    <dependencies>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.1.3</version>
        </dependency>
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-core</artifactId>
            <version>1.3.2</version>
        </dependency>
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-web</artifactId>
            <version>1.3.2</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```



##### 1.1.3 web.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">
    <display-name>shiro-07web</display-name>

    <!-- 初始化SecurityManager对象所需要的环境-->
    <context-param>
        <param-name>shiroEnvironmentClass</param-name>
        <param-value>org.apache.shiro.web.env.IniWebEnvironment</param-value>
    </context-param>

    <!-- 指定Shiro的配置文件的位置 -->
    <context-param>
        <param-name>shiroConfigLocations</param-name>
        <param-value>classpath:shiro.ini</param-value>
    </context-param>

    <!-- 监听服务器启动时，创建shiro的web环境。
         即加载shiroEnvironmentClass变量指定的IniWebEnvironment类-->
    <listener>
        <listener-class>org.apache.shiro.web.env.EnvironmentLoaderListener</listener-class>
    </listener>

    <!-- shiro的l拦截入口，拦截一切请求 -->
    <filter>
        <filter-name>shiroFilter</filter-name>
        <filter-class>org.apache.shiro.web.servlet.ShiroFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>shiroFilter</filter-name>
        <!-- 拦截所有请求 -->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

#### 1.2 SecurityManager对象创建



### 2、Shiro默认过滤器

Shiro内置了很多默认的过滤器，比如身份验证、授权等相关的。默认过滤器可以参考org.apache.shiro.web.filter.mgt.DefaultFilter中的枚举过滤器

```java
package org.apache.shiro.web.filter.mgt;    
public enum DefaultFilter {
    anon(AnonymousFilter.class),
    authc(FormAuthenticationFilter.class),
    authcBasic(BasicHttpAuthenticationFilter.class),
    logout(LogoutFilter.class),
    noSessionCreation(NoSessionCreationFilter.class),
    perms(PermissionsAuthorizationFilter.class),
    port(PortFilter.class),
    rest(HttpMethodPermissionFilter.class),
    roles(RolesAuthorizationFilter.class),
    ssl(SslFilter.class),
    user(UserFilter.class);
}
```



#### 2.1 认证相关

| 过滤器     | 过滤器类                      | 说明                                                         | 默认 |
| ---------- | ----------------------------- | ------------------------------------------------------------ | ---- |
| authc      | FormAuthenticationFilter      | 基于表单的过滤器；如/**=auth，如果没有登录会跳到响应的登录也面登录 | 无   |
| logout     | LogoutFilter                  | 退出过滤器，主要属性：redirectUrl：退出成功后重定向的地址，如/logout=logout | /    |
| anon       | AnonymousFilter               | 匿名过滤器，即不需要登录也可以访问；一般用于静态资源过滤，示例："/static/**=anon" | 无   |
| authcBasic | BasicHttpAuthenticationFilter | BaseHTTP身份验证过滤器，主要属性：applicationName：弹出框显示的信息（aoolication） |      |
| user       | UserFilter                    | 用户过滤器，用户已经身份验证/记住我都可以，示例："/**=user"  |      |



#### 2.2 授权相关

| 过滤器 | 过滤器类                       | 说明                                                         | 默认 |
| ------ | ------------------------------ | ------------------------------------------------------------ | ---- |
| roles  | RolesAuthorizationFilter       | 角色授权过滤器，验证用户是否拥有角色；主要属性：loginUrl：登录页面地址(/login.jsp);  unauthorizedUrl：未授权重定向地址；示例："/admin/**=roles[admin]" |      |
| perms  | PermissionsAuthorizationFilter | 授权过滤器，验证用户是否拥有权限，属性和Roles一样；示例："/user/**=parms["user:create"]" |      |
| port   | PortFilter                     | 端口过滤器，主要属性port（80），表示可以通过的端口；示例："/test=port[80]",如果用户访问的页面是非80，会自动将端口改为80端口，其他路径参数都一样 |      |
| rest   | HttpMethodPermissionFilter     | reset风格过滤器，会自动根据请求方法构建权限字符串            |      |
| ssl    | SslFilter                      | SSL过滤器，只有请求协议是https才能通过；否则会自动跳转到https端口（443）；其他和port过滤器一样 |      |

### 3、Web集成完整案例

#### 3.1 编写pom.xml

```xml
<dependencies>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.1.3</version>
        </dependency>
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-core</artifactId>
            <version>1.3.2</version>
        </dependency>
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-web</artifactId>
            <version>1.3.2</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <dependency>
            <groupId>taglibs</groupId>
            <artifactId>standard</artifactId>
            <version>1.1.2</version>
        </dependency>
    </dependencies>
```



#### 3.2 编写shiro.ini

```ini
#声明自定义的realm,且为安全管理器指定realms
[main]
definitionRealm=com.beyondsoft.shiro.realm.DefinitionRealm
securityManager.realms=$definitionRealm
#用户退出后跳转到指定JSP云
logout.redirectUrl=/login.jsp
#若没有登录，则被authc过滤器重定向到login.jsp页面
authc.loginUrl=/login.jsp
[urls]
/login=anon
#发送/home请求需要先登录
/home=authc
/order-list=roles[admin]
/order-add=perms["order:add"]
/order-del=perms["order:del"]
/logout=logout
```

#### 3.3 编写LoginService

LoginService

```java
package com.beyondsoft.shiro.service;

import org.apache.shiro.authc.UsernamePasswordToken;

public interface LoginService {

    /**
     * 登录方法
     * @param token
     * @return
     */
    boolean login(UsernamePasswordToken token);

    /**
     * 退出
     */
    void logout();
}
```

LoginServiceImpl

```java
package com.beyondsoft.shiro.service.impl;

import com.beyondsoft.shiro.service.LoginService;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.subject.Subject;

/**
 * 登录服务实现
 */
public class LoginServiceImpl implements LoginService {

    @Override
    public boolean login(UsernamePasswordToken token) {
        Subject subject = SecurityUtils.getSubject();
        try {
            subject.login(token);
        } catch (Exception e) {
            return false;
        }
        return subject.isAuthenticated();
    }

    @Override
    public void logout() {
        Subject subject = SecurityUtils.getSubject();
        subject.logout();
    }
}
```

#### 3.4 编写SecurityServiceImpl

```java
package com.beyondsoft.shiro.service.impl;

import com.beyondsoft.shiro.service.SecurityService;
import com.beyondsoft.shiro.util.DigestsUtil;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;

/**
 * 模拟数据库服务接口的实现
 */
public class SecurityServiceImpl implements SecurityService {

    @Override
    public Map<String,String> findPassByLoginName(String loginName) {
        return DigestsUtil.entryptPassword("123") ;
    }

    @Override
    public List<String> findRoleByLoginName(String loginName) {
        List<String> roles = new ArrayList<>();
        if("admin".equals(loginName)){
            roles.add("admin");
        }
        roles.add("dev");
        return roles;
    }

    @Override
    public List<String> findPermissionByLoginName(String loginName) {
        List<String> list = new ArrayList<>();
        if("jay".equals(loginName)) {
            list.add("order:add");
            list.add("order:list");
            list.add("order:delete");
        }
        return list;
    }
}
```

#### 3.5 添加web层内容

##### 3.5.1 LoginServlet

```java
package com.beyondsoft.shiro.web;

import com.beyondsoft.shiro.service.LoginService;
import com.beyondsoft.shiro.service.impl.LoginServiceImpl;
import org.apache.shiro.authc.UsernamePasswordToken;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(urlPatterns = "/login")
public class LoginServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获得用户名和密码
        String username= req.getParameter("loginname");
        String password = req.getParameter("password");
        //构建登录使用的token
        UsernamePasswordToken token = new UsernamePasswordToken(username,password);
        //登录操作
        LoginService loginService = new LoginServiceImpl();
        boolean isLogin = loginService.login(token);
        if(isLogin) {
            //如果登录成功 跳转home.jsp
            req.getRequestDispatcher("/home").forward(req,resp);
        } else {
            //如果登录失败，跳转继续登录页面
            resp.sendRedirect("login.jsp");
        }
    }
}
```

##### 3.5.2 HomeServlet

```java
package com.beyondsoft.shiro.web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(urlPatterns = "/home")
public class HomeServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getRequestDispatcher("home.jsp").forward(req,resp);
    }
}
```

##### 3.5.3 OrderAddServlet

```java
package com.beyondsoft.shiro.web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(urlPatterns = "/order-add")
public class OrderAddServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getRequestDispatcher("order-add.jsp").forward(req,resp);
    }
}
```

##### 3.5.4 OrderListServlet

```java
package com.beyondsoft.shiro.web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(urlPatterns = "/order-list")
public class OrderListServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getRequestDispatcher("order-list.jsp").forward(req,resp);
    }
}
```

##### 3.5.5 LogoutServlet

```java
package com.beyondsoft.shiro.web;

import com.beyondsoft.shiro.service.LoginService;
import com.beyondsoft.shiro.service.impl.LoginServiceImpl;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class LogoutServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        LoginService loginService = new LoginServiceImpl();
        loginService.logout();
    }
}
```

#### 3.6 添加jsp

login.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body>
<form method="post" action="${pageContext.request.contextPath}/login">
    <table>
        <tr>
            <td>登录名称</td>
            <td><input type="text" name="loginname"></td>
        </tr>
        <tr>
            <td>密码</td>
            <td><input type="text" name="password"></td>
        </tr>
        <tr>
            <td colspan="2">
                <input type="submit" value="提交">

            </td>
        </tr>
    </table>
</form>
</body>
</html>
```

home.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h6>
    <a href="${pageContext.request.contextPath}/logout">退出</a>
    <a href="${pageContext.request.contextPath}/order-list">列表</a>
    <a href="${pageContext.request.contextPath}/order-add">添加</a>
</h6>
</body>
</html>

```

order-add.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
添加页面
</body>
</html>
```

order-list.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <style>
        table{border:1px solid #000000}
        table th{border:1px solid #000000}
        table td{border:1px solid #000000}
    </style>
</head>
<body>
<table cellpadding="0" cellspacing="0" width="80%">
    <tr>
        <th>编号</th>
        <th>公司名称</th>
        <th>信息来源</th>
        <th>所属行业</th>
        <th>级别</th>
        <th>联系地址</th>
        <th>联系电话</th>
    </tr>
    <tr>
        <td>1</td>
        <td>黑马程序员</td>
        <td>网络营销</td>
        <td>互联网</td>
        <td>普通客户</td>
        <td>津安创意园</td>
        <td>1234567890</td>
    </tr>
</table>
</body>
</html>

```



#### 3.7 测试

##### 3.7.1 启动

```
http://localhost:8080/shiro-07web/login.jsp
```

##### 3.7.1 登录过滤

```
http://localhost:8080/shiro-07web/home
```

##### 3.7.3 角色过滤

使用admin 密码123 登录

```
可以访问
http://localhost:8080/shiro-07web/order-list
不能访问
http://localhost:8080/shiro-07web/order-add
```

##### 3.7.4 资源过滤

使用jay 密码123登录

```
不能访问 报401权限错误
http://localhost:8080/shiro-07web/order-list
不能访问
http://localhost:8080/shiro-07web/order-add
可以访问
http://localhost:8080/shiro-07web/home
```



### 4、Web项目授权

前面我们学习了基于ini文件配置方式来完成首期，下面我们开看下其他的两种方式的授权

#### 4.1 基于代码

##### 4.1.1 登录相关

| Subject角色相方法 | 描述                                |
| ----------------- | ----------------------------------- |
| isAuthenticated() | 返回true表示已经登录，否则返回false |



##### 4.1.2 角色相关

| Subject角色相方法                         | 描述                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| hasRole(String roleName)                  | 返回true 如果Subject被分配了指定的角色，否则返回false        |
| hasRoles(List<String>roleNames)           | 返回true 如果Subject 被分配了所有指定的角色，否则返回false。 |
| hasAllRoles(Collection<String> roleNames) | 返回一个与方法参数中目录一致的hasRole 结果的集合。           |
| checkRole(String roleName)                | 安静地返回，如果Subject被分配了指定的角色，不的话就会抛出AuthorizationException |
| checkRoles(Collection<String> roleNames)  | 安静地返回，如果Subject 被分配了所有的指定的角色，不然的话就抛出AuthorizationException |
| checkRoles(String... roleNames)           | 与上面的checkRoles 方法的效果相同，但允许Java5 的var-args 类型的参数。 |

##### 4.1.3 资源相关

| Subject角色相方法                             | 描述                                                         |
| --------------------------------------------- | ------------------------------------------------------------ |
| isPermitted(Permissionp)                      | 返回true如果该Subject被允许执行某动作或访问被权限实例指定的资源集合，否则返回false。 |
| isPermitted(List<Permission>perms)            | 返回一个与方法参数中目录一致的isPermitted结果的数组。        |
| isPermittedAll(Collection<Permission>perms)   | 返回true如果该Subject被允许所有指定的权限，否则返回false。   |
| checkPermission(Permissionp)                  | 安静地返回，如果Subject被允许执行某动作或访问被特定的权限实例指定的资源，不然的话就抛出AuthorizationException异常。 |
| checkPermission(Stringperm)                   | 安静地返回，如果Subject被允许执行某动作或访问被特定的字符串权限指定的资源，不然的话就抛出AuthorizationException异常。 |
| checkPermissions(Collection<Permission>perms) | 安静地返回，如果Subject被允许所有的权限，不然的话就抛出AuthorizationException异常。 |
| checkPermissions(String…perms)                | 和上面的checkPermissions方法效果相同，但是使用的是基于字符串的权限。 |

##### 4.1.4 案例

###### 4.1.4.1 创建项目

shiro-08web-java 从shiro-07web 项目拷贝

###### 4.1.4.2 修改shiro.ini

注释路径过滤器相关配置

```ini
#声明自定义的realm,且为安全管理器指定realms
[main]
definitionRealm=com.beyondsoft.shiro.realm.DefinitionRealm
securityManager.realms=$definitionRealm
#用户退出后跳转到指定JSP云
logout.redirectUrl=/login.jsp
#若没有登录，则被authc过滤器重定向到login.jsp页面
authc.loginUrl=/login.jsp
[urls]
/login=anon
#发送/home请求需要先登录
#/home=authc
#/order-list=roles[admin]
#/order-add=perms["order:add"]
#/order-del=perms["order:del"]
/logout=logout

```

###### 4.1.4.3 登录相关

HomeServlet

```java
package com.beyondsoft.shiro.web;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.subject.Subject;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(urlPatterns = "/home")
public class HomeServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获得当前主体
        Subject subject = SecurityUtils.getSubject();
        //获取当前主体是否登录
        boolean authenticated = subject.isAuthenticated();
        if(authenticated) {
            req.getRequestDispatcher("home.jsp").forward(req, resp);
        }
        req.getRequestDispatcher("/login").forward(req, resp);
    }
}
```

###### 4.1.4.4 角色相关

OrderListServlet

```java
package com.beyondsoft.shiro.web;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.subject.Subject;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(urlPatterns = "/order-list")
public class OrderListServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获得当前主体
        Subject subject = SecurityUtils.getSubject();
        //判断当前主体是否具有admin角色
        boolean isAdmin = subject.hasRole("admin");
        if(isAdmin) {
            req.getRequestDispatcher("order-list.jsp").forward(req, resp);
            return;
        }
        //权限不足 提示401错误 或者返回登录
        req.getRequestDispatcher("/login").forward(req, resp);
    }
}
```

###### 4.1.4.5 资源相关

OrderAddServlet

```java
package com.beyondsoft.shiro.web;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.subject.Subject;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(urlPatterns = "/order-add")
public class OrderAddServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获得当前主体
        Subject subject = SecurityUtils.getSubject();
        //判断当前主体是否有相应的权限
        boolean permitted = subject.isPermitted("order:add");
        if(permitted) {
            req.getRequestDispatcher("order-add.jsp").forward(req, resp);
            return;
        }
        //权限不足 提示401错误 或者返回登录
        req.getRequestDispatcher("/login").forward(req, resp);
    }
}
```

#### 4.2 基于jsp标签

###### 4.2.1 使用方式

Shiro提供了一套JSP标签库来实现页面级的授权控制，在使用Shiro标签库前，首先需要在Jsp页面引入shiro标签

```jsp
<%@ taglib prefix="shiro" uri="http://shiro.apache.org/tags" %>
```

###### 4.2.2 相关标签

| 标签                                 | 说明                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| \<shiro:guest>                       | 验证当前用户是否为“访客”，即未认证(包含为记住)的用户         |
| \<shiro:user>                        | 认证通过或已记住的用户                                       |
| \<shiro:authenticated>               | 已认证通过的用户，不包含已经记住的用户，这是与user标签的区别所在 |
| \<shiro:notAuthenticated>            | 未认证通过的用户，与guest标签的区别是该标签包含已记住用户    |
| \<shiro:principal>                   | 输出当前用户信息，通常为登录账号信息                         |
| \<shiro:hasRole name="角色">         | 验证当前用户是否具有该角色                                   |
| \<shiro:lacksRole name="角色">       | 与hasRole标签逻辑相反，当用户不具有该角色时验证通过          |
| \<shiro:hasRoles name="a,b">         | 验证当前用户是否具有以下任意一个角色                         |
| \<shiro:hasPermission name="资源">   | 验证当前用户是否拥有指定的权限                               |
| \<shiro:lacksPermission name="资源"> | 与shiro:hasPermission标签逻辑相反，验证当前用户是否不具有指定权限 |

###### 4.2.3 案例

新建项目shiro-09web-jsp-taglib，从shiro-008web-java项目拷贝

修改home.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="shiro" uri="http://shiro.apache.org/tags" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h6>
    <a href="${pageContext.request.contextPath}/logout">退出</a>
    <shiro:hasRole name="admin">
    <a href="${pageContext.request.contextPath}/order-list">列表</a>
    </shiro:hasRole>
    <shiro:hasPermission name="order:add">
    <a href="${pageContext.request.contextPath}/order-add">添加</a>
    </shiro:hasPermission>
</h6>
</body>
</html>

```

注意：jsp 标签只能控制页面控件的显示与否，不能防止盗链。安全级别较低

#### 4.3 基于注解的方式

依赖于spring，我们将在下面章节进行介绍。



## 第五章 SpringBoot项目集成Shiro

### 1、技术栈

主框架：Springboot

响应层：SpringMVC

持久层：Mybatis

事务控制：JTA

前端框架：easyUI

### 2、数据库设计

#### 2.1 数据库图解

![img](https://img-blog.csdnimg.cn/img_convert/d47b3b894ac3a6ad076e875ec3065f6d.png)

sh_user:  用户表，一个用户可以有多个角色

sh_role:  角色表，一个角色可以有多个资源

sh_resource:  资源表

sh_user_role:  用户角色中间表

sh_role_resource:  角色资源中间表

#### 2.2 数据库脚本

sh_user

```sql
CREATE TABLE `sh_user` (  `ID` varchar(36) NOT NULL COMMENT '主键',  `LOGIN_NAME` varchar(36) DEFAULT NULL COMMENT '登录名称',  `REAL_NAME` varchar(36) DEFAULT NULL COMMENT '真实姓名',  `NICK_NAME` varchar(36) DEFAULT NULL COMMENT '昵称',  `PASS_WORD` varchar(150) DEFAULT NULL COMMENT '密码',  `SALT` varchar(36) DEFAULT NULL COMMENT '加密因子',  `SEX` int(11) DEFAULT NULL COMMENT '性别',  `ZIPCODE` varchar(36) DEFAULT NULL COMMENT '邮箱',  `ADDRESS` varchar(36) DEFAULT NULL COMMENT '地址',  `TEL` varchar(36) DEFAULT NULL COMMENT '固定电话',  `MOBIL` varchar(36) DEFAULT NULL COMMENT '电话',  `EMAIL` varchar(36) DEFAULT NULL COMMENT '邮箱',  `DUTIES` varchar(36) DEFAULT NULL COMMENT '职务',  `SORT_NO` int(11) DEFAULT NULL COMMENT '排序',  `ENABLE_FLAG` varchar(18) DEFAULT NULL COMMENT '是否有效',  PRIMARY KEY (`ID`)) ENGINE=InnoDB DEFAULT CHARSET=utf8 ROW_FORMAT=COMPACT COMMENT='用户表';
```

sh_role

```sql
CREATE TABLE `sh_role` (  `ID` varchar(36) NOT NULL COMMENT '主键',  `ROLE_NAME` varchar(36) DEFAULT NULL COMMENT '角色名称',  `LABEL` varchar(36) DEFAULT NULL COMMENT '角色标识',  `DESCRIPTION` varchar(200) DEFAULT NULL COMMENT '角色描述',  `SORT_NO` int(36) DEFAULT NULL COMMENT '排序',  `ENABLE_FLAG` varchar(18) DEFAULT NULL COMMENT '是否有效',  PRIMARY KEY (`ID`)) ENGINE=InnoDB DEFAULT CHARSET=utf8 ROW_FORMAT=COMPACT COMMENT='用户角色表';
```

sh_resource

```sql
CREATE TABLE `sh_resource` (  `ID` varchar(36) NOT NULL COMMENT '主键',  `PARENT_ID` varchar(36) DEFAULT NULL COMMENT '父资源',  `RESOURCE_NAME` varchar(36) DEFAULT NULL COMMENT '资源名称',  `REQUEST_PATH` varchar(200) DEFAULT NULL COMMENT '资源路径',  `LABEL` varchar(200) DEFAULT NULL COMMENT '资源标签',  `ICON` varchar(20) DEFAULT NULL COMMENT '图标',  `IS_LEAF` varchar(18) DEFAULT NULL COMMENT '是否叶子节点',  `RESOURCE_TYPE` varchar(36) DEFAULT NULL COMMENT '资源类型',  `SORT_NO` int(11) DEFAULT NULL COMMENT '排序',  `DESCRIPTION` varchar(200) DEFAULT NULL COMMENT '描述',  `SYSTEM_CODE` varchar(36) DEFAULT NULL COMMENT '系统code',  `IS_SYSTEM_ROOT` varchar(18) DEFAULT NULL COMMENT '是否根节点',  `ENABLE_FLAG` varchar(18) DEFAULT NULL COMMENT '是否有效',  PRIMARY KEY (`ID`)) ENGINE=InnoDB DEFAULT CHARSET=utf8 ROW_FORMAT=COMPACT COMMENT='资源表';
```

sh_role_resource

```sql
CREATE TABLE `sh_role_resource` (  `ID` varchar(36) NOT NULL,  `ENABLE_FLAG` varchar(18) DEFAULT NULL,  `ROLE_ID` varchar(36) DEFAULT NULL,  `RESOURCE_ID` varchar(36) DEFAULT NULL,  PRIMARY KEY (`ID`)) ENGINE=InnoDB DEFAULT CHARSET=utf8 ROW_FORMAT=COMPACT COMMENT='角色资源表';
```

sh_user_role

```sql
CREATE TABLE `sh_user_role` (  `ID` varchar(36) NOT NULL,  `ENABLE_FLAG` varchar(18) DEFAULT NULL,  `USER_ID` varchar(36) DEFAULT NULL,  `ROLE_ID` varchar(36) DEFAULT NULL,  PRIMARY KEY (`ID`)) ENGINE=InnoDB DEFAULT CHARSET=utf8 ROW_FORMAT=COMPACT COMMENT='用户角色表';
```

### 3、项目骨架

![c116b4a7d0d7856bb91b9f76726ff3a9.png](https://img-blog.csdnimg.cn/img_convert/c116b4a7d0d7856bb91b9f76726ff3a9.png)

### 4、ShiroDbRealm定义

#### 4.1 图解

![c39e4012a3c3ae3ee2e0c98734703b4f.png](https://img-blog.csdnimg.cn/img_convert/c39e4012a3c3ae3ee2e0c98734703b4f.png)

#### 4.2 原理分析

(1)、ShiroDbRealmImpl继承ShiroDbRealm向上继承AuthorizingRealm，ShiroDbRealmImpl实例化时会创建密码匹配器HashedCredentialsMatcher实例，HashedCredentialsMatcher指定hash次数与方式，交于AuthenticatingRealm

(2)、调用login方法后，最终调用doGetAuthenticationInfo(AuthenticationToken authcToken)方法，拿到SimpleToken的对象，调用UserBridgeService的查找用户方法，把ShiroUser对象、密码和salt交于SimpleAuthenticationInfo去认证

(3)、访问需要鉴权时，调用doGetAuthorizationInfo(PrincipalCollection principals)方法，然后调用UserBridgeService的授权验证

#### 4.3 核心类代码

##### 4.3.1 ShiroDbRealm

```java
package com.itheima.shiro.core;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;import org.apache.shiro.authz.AuthorizationInfo;import org.apache.shiro.realm.AuthorizingRealm;import org.apache.shiro.subject.PrincipalCollection;import javax.annotation.PostConstruct;/** * * @Description shiro自定义realm */public abstract class ShiroDbRealm extends AuthorizingRealm {    /**     * @Description 认证     * @param authcToken token对象     * @return      */    public abstract AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authcToken) ;    /**     * @Description 鉴权     * @param principals 令牌     * @return     */    public abstract AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals);    /**     * @Description 密码匹配器     */    @PostConstruct    public abstract void initCredentialsMatcher() ;}
```

##### 4.3.2 ShiroDbRealmImpl

```java
package com.itheima.shiro.core.impl;import com.itheima.shiro.constant.SuperConstant;import com.itheima.shiro.core.base.ShiroUser;import com.itheima.shiro.core.base.SimpleToken;import com.itheima.shiro.core.ShiroDbRealm;import com.itheima.shiro.core.bridge.UserBridgeService;import com.itheima.shiro.pojo.User;import com.itheima.shiro.utils.BeanConv;import com.itheima.shiro.utils.DigestsUtil;import com.itheima.shiro.utils.EmptyUtil;import org.apache.shiro.authc.AuthenticationInfo;import org.apache.shiro.authc.AuthenticationToken;import org.apache.shiro.authc.SimpleAuthenticationInfo;import org.apache.shiro.authc.UnknownAccountException;import org.apache.shiro.authc.credential.HashedCredentialsMatcher;import org.apache.shiro.authz.AuthorizationInfo;import org.apache.shiro.subject.PrincipalCollection;import org.apache.shiro.util.ByteSource;import org.springframework.beans.factory.annotation.Autowired;/** * @Description：自定义shiro的实现 */public class ShiroDbRealmImpl extends ShiroDbRealm {    @Autowired    private UserBridgeService userBridgeService;    /**     * @Description 认证方法     * @param authcToken 校验传入令牌     * @return AuthenticationInfo     */    @Override    public AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authcToken) {        SimpleToken token = (SimpleToken)authcToken;        User user  = userBridgeService.findUserByLoginName(token.getUsername());        if(EmptyUtil.isNullOrEmpty(user)){            throw new UnknownAccountException("账号不存在");        }        ShiroUser shiroUser = BeanConv.toBean(user, ShiroUser.class);        shiroUser.setResourceIds(userBridgeService.findResourcesIdsList(user.getId()));        String salt = user.getSalt();        String password = user.getPassWord();        return new SimpleAuthenticationInfo(shiroUser, password, ByteSource.Util.bytes(salt), getName());    }    /**     * @Description 授权方法     * @param principals SimpleAuthenticationInfo对象第一个参数     * @return     */    @Override    public AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {        ShiroUser shiroUser = (ShiroUser) principals.getPrimaryPrincipal();        return userBridgeService.getAuthorizationInfo(shiroUser);    }    /**     * @Description 加密方式     */    @Override    public void initCredentialsMatcher() {        HashedCredentialsMatcher matcher = new HashedCredentialsMatcher(SuperConstant.HASH_ALGORITHM);        matcher.setHashIterations(SuperConstant.HASH_INTERATIONS);        setCredentialsMatcher(matcher);    }}
```

##### 4.3.3 SimpleToken

```
package com.itheima.shiro.core.base;
import org.apache.shiro.authc.UsernamePasswordToken;
/** * @Description 自定义tooken */
public class SimpleToken extends UsernamePasswordToken {
/** serialVersionUID */    private static final long serialVersionUID = -4849823851197352099L;    private String tokenType;    private String quickPassword;    /**     * Constructor for SimpleToken     * @param tokenType     */    public SimpleToken(String tokenType, String username,String password) {        super(username,password);        this.tokenType = tokenType;    }    public SimpleToken(String tokenType, String username,String password,String quickPassword) {        super(username,password);        this.tokenType = tokenType;        this.quickPassword = quickPassword;    }    public String getTokenType() {        return tokenType;    }    public void setTokenType(String tokenType) {        this.tokenType = tokenType;    }    public String getQuickPassword() {        return quickPassword;    }    public void setQuickPassword(String quickPassword) {        this.quickPassword = quickPassword;    }}
```

### 4、ShiroDbRealm定义

### 5、ShiroConfig配置

5.1 图解

5.2 原理分析

（1）、创建SimpleCookie，访问项目时，会在客户端中的cookie中存放ShiroSession的对

（2）、创建DefaultWebSessionManager会话管理器定义cookie机制，定时刷新、全局会话超时时间然后交

### 6、Shiro过滤器

### 7、注解方式鉴权

### 8、项目测试





## 第六章 实现Realm的缓存机制



## 第七章 实现分布式会话SessionManager



## 第八章 限制密码重试次数



## 第九章 在线并发登录人数限制



### 第十章 Springboot+Shiro+Jwt前后端分离鉴权