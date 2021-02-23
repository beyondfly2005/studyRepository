## 2020年Java视频教程-Shiro全集

> 视频地址：https://www.bilibili.com/video/BV1h7411p7uj
>
> 

## 一、Shiro

**为什么要用shiro？**

**1.项目中的密码是否可以明文存储？**

**2.是否任意访客，无论是否登录都可以访问任何功能?**

**3.项目中的各种功能操作，是否是所有用户都可以随意使用？**

**综上，当项目中的某些功能被使用时，需要进行安全校验，进而保证整个系统的运行秩序。**

### 1.1 Shiro是什么

```
• Apache Shiro 是 Java 的一个安全（权限）框架。
  Shiro 可以轻松的完成：身份认证、授权、加密、会话管理等功能
• Shiro 可以非常容易的开发出足够好的应用，其不仅可以用在JavaSE 环境，也可以用在 JavaEE 环境。
  功能强大且易用，可以快速轻松地保护任何应用程序 ( 从最小的移动应用程序到最大的Web和企业应用程序。)
• 方便的与Web 集成和搭建缓存。
• 下载：http://shiro.apache.org/
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191010075113231.jpg)

### 1.2 功能简介

• **基本功能点如下图所示：**

![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-HzYawvMd-1570664998281)(mdpic\2.jpg)]](https://img-blog.csdnimg.cn/20191010075122403.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MDg1OTU0,size_16,color_FFFFFF,t_70)

```markdown
• Authentication
	身份认证/登录，验证用户是不是拥有相应的身份；
• Authorization
	授权，即权限验证，验证某个已认证的用户是否拥有某个权限；即判断用户是否能进行什么操作
	如：验证某个用户是否拥有某个角色。或者细粒度的验证某个用户对某个资源是否具有某个权限；
• Session Manager
	会话管理，即用户登录后就是一次会话，在没有退出之前，它的所有
    信息都在会话中；会话可以是普通 JavaSE 环境，也可以是 Web 环境的；
• Cryptography
	加密，保护数据的安全性，如密码加密存储到数据库，而不是明文存储；
• Web Support
	Web 支持，可以非常容易的集成到Web 环境=；
• Caching
	缓存，比如用户登录后，其用户信息、拥有的角色/权限不必每次去查，这样可以提高效率；
• Remember Me
	记住我，这个是非常常见的功能，即一次登录后，下次再来的话可以立即知道你是哪个用户
。。。。
```

## 二、Shiro 架构

### 2.1 工作流程

> **shiro 运行流程中，3个核心的组件:**
>
> #### **`Subject、SecurityManager、Realm`**
>
> **三大金刚：SSR**
>
> #### **shiro使用中，必须有的3个概念！！**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191010075142445.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MDg1OTU0,size_16,color_FFFFFF,t_70)

```markdown
• Subject
	安全校验中，最常见的问题是"当前用户是谁?" "当前用户是否有权做x事?",所以考虑安全校验过程最自
    然的方式就是基于当前用户。Subject 代表了当前“用户”，
    应用代码直接交互的对象是 Subject，只要得到Subject对象马上可以做绝大多数的shiro操作。
    也就是说 Shiro 的对外API 核心就是 Subject。
    Subject 会将所有交互都会委托给 SecurityManager。
    ==Subject是安全管理中直接操作的对象==

• SecurityManager【老大】
	安全管理器；即所有与安全有关的操作都会与SecurityManager 交互；
	且其管理着所有 Subject；它是 Shiro的核心，
	它负责与 Shiro 的其他组件进行交互，它相当于 SpringMVC 中DispatcherServlet 的角色

• Realm
	Shiro 从 Realm 获取安全数据（如用户、角色、权限），就是说SecurityManager 要验证用户身份，那么 
	它需要从 Realm 获取相应的用户进行比较以确定用户身份是否合法；
	也需要从 Realm 得到用户相应的角色/权限进行验证用户是否能进行操作；
	可以把 Realm 看成 DAO，( 数据访问入口 )
123456789101112131415161718
```

### 2.2 RBAC模型

> #### **Role Base Access Controll 基于角色的访问控制**

> #### **shiro采用的安全管理 模型**

> #### **模型中3个主体：用户、角色、权限**
>
> **每个角色可以有多个权限，每个权限可以分配给多个角色**
>
> **每个用户可以有多个角色，每个角色可以分配给多个用户**
>
> ***两个多对多***
>
> **【5张表】**

![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-BLw4JQ5S-1570664998282)(mdpic/rbac1.jpg)]](https://img-blog.csdnimg.cn/20191010075210607.jpg)

> **则权限访问控制，做的事是：**
>
> 1. **身份校验：判断是否为合法用户**
> 2. **权限校验：用户要做做某件事或使用某些资源，必须要拥有某角色，或必须拥有某权限**

> **在访问控制管理过程中，是要对项目中的资源(功能，数据，页面元素等)的访问、使用进行安全管理。**
>
> **1> 首先照旧记录用户信息**
>
> **2> 然后制定角色**
>
>  如 `“千锋学员”，“千锋讲师”，“保洁员”`
>
> **3> 然后会对"资源"制定权限：即能对资源做的所有操作**
>
>  如 `"教室"`-资源, `"进入教室"`，`“在教室学习”`，就是该资源的两个权限。
>
> **4> 然后将权限分配给不同角色**
>
>  如 `“进入教室”` 分配给 `“千锋学员”，“千锋讲师”，“保洁员”` 三种角色
>
>  `“在教室学习”` 分配给 `“千锋学员”` 角色
>
> **5> 最后将角色分配给具体用户**
>
>  如 `“张三”` 报名后分配 `“千锋学员”`角色
>
>  `“李四”`入职千锋分配`"千锋保洁员"`角色
>
> **6> 如此完成对 用户(李四，张三)使用某资源的访问控制**
>
>  则：张三能不能进教室？李四能不能在教室学习？

### 2.3 架构

> **简单了解，在对shiro有了完整的应用体验后可以重点了解！**

![img](https://img-blog.csdnimg.cn/20191010075227437.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MDg1OTU0,size_16,color_FFFFFF,t_70)

```markdown
• Subject【一个请求一份】
	任何可以与应用交互的“用户”；
• SecurityManager 
	相当于SpringMVC 中的 DispatcherServlet；是 Shiro 的心脏；
	所有具体的交互都通过 SecurityManager 进行控制；它管理着所有 Subject、且负责进行认证、授权、会话及
	缓存的管理。
• Authenticator
	负责 Subject 身份认证，是一个扩展点，可以自定义实现；可以使用认证
	策略（Authentication Strategy），即什么情况下算用户认证通过了；
• Authorizer
	授权器、即访问控制器，用来决定主体是否有权限进行相应的操作；即控制着用户能访问应用中的哪些功能；
• Realm
	可以有 1 个或多个 Realm，可以认为是安全实体数据源，即用于获取安全实体
	的；可以是JDBC 实现，也可以是内存实现等等；由用户提供；所以一般在应用中都需要
	实现自己的 Realm；
• SessionManager
	管理 Session 生命周期的组件；而 Shiro 并不仅仅可以用在 Web环境，也可以用在如普通的 JavaSE 环境
• CacheManager
	缓存控制器，来管理如用户、角色、权限等的缓存的；因为这些数据基本上很少改变，
	放到缓存中后可以提高访问的性能
• Cryptography
	密码模块，Shiro 提供了一些常见的加密组件用于如密码加密/解密。
```

## 三、HelloWorld

### 3.1 pom 文件

```xml
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-core</artifactId>
    <version>1.4.0</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.12</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.7.12</version>
</dependency>
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.16</version>
</dependency>
<dependency>
    <groupId>commons-logging</groupId>
    <artifactId>commons-logging</artifactId>
    <version>1.2</version>
</dependency>
12345678910111213141516171819202122232425
```

> **ops : require JDK 1.6+ and Maven 3.0.3+**

### 3.2 配置

> 创建一个`shiro.ini`文件

```ini
#定义用户信息
#格式：用户名=密码,角色1,角色2,....
[users]
zhangsan=123,admin
lisi=456,manager,seller
wangwu=789,clerk
# -----------------------------------------------------------------------------
# 角色及其权限信息
# 预定权限： user:query
#          user:detail:query
#          user:update
#          user:delete
#          user:insert
#          order:update
#          ....
[roles]
# admin 拥有所有权限,用*表示
admin=*
# clerk 只有查询权限
clerk=user:query,user:detail:query
# manager 有 user 的所有权限
manager=user:*
12345678910111213141516171819202122
```

### 3.3 代码

> - 通过在ini中设置的 身份信息、角色、权限，做安全管理
> - 1.身份认证: subject.login(token)
> - 2.是否认证身份判断：subject.isAuthenticated()
> - 3.角色校验：subject.hasRole(String:role); subject.hasRoles(List:roles)
> - 4.权限校验：subject.isPermitted(String:perm); subject.isPermittedAll(List:perms)

```java
// 定义main函数测试效果
// 创建 "SecurityFactory",加载ini配置,并通过它创建SecurityManager 
Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
SecurityManager securityManager = factory.getInstance();

// 将SecurityManager托管到SecurityUtils工具类中(ops:之后可以不必关心SecurityManager)
SecurityUtils.setSecurityManager(securityManager);

// 获得Subject，通过subject可以执行shiro的相关功能操作(身份认证或权限校验等)
Subject currentUser = SecurityUtils.getSubject();

// 身份认证( 类似登录逻辑 )
if (!currentUser.isAuthenticated()) {//判断是否已经登录
    //如果未登录，则封装一个token，其中包含 用户名和密码
    UsernamePasswordToken token = new UsernamePasswordToken("zhangsan", "123");
    try {
        //将token传入login方法，进行身份认证 (判断用户名和密码是否正确)
        currentUser.login(token);//如果失败则会抛出异常
    } catch (UnknownAccountException uae) {//用户不存在
        System.out.println("There is no user with username of " + token.getPrincipal());
    } catch (IncorrectCredentialsException ice) {//密码错误
        System.out.println("Password for account " + token.getPrincipal() + " was incorrect!");
    } catch (LockedAccountException lae) {//账户冻结，手动抛出
        System.out.println("The account for username " + token.getPrincipal() + " is locked.  " 
                           +"Please contact your administrator to unlock it.");
    } catch (AuthenticationException ae) {//其他认证异常
        
    }
}

// 认证成功则用户信息会存入 currentUser（Subject）
System.out.println("User [" + currentUser.getPrincipal() + "] logged in successfully.");

// 可以进一步进行角色校验 和 权限校验
if (currentUser.hasRole("admin")) { //校验角色
    System.out.println("hello,boss");
} else {
    System.out.println("Hello, you");
}
if (currentUser.isPermitted("user:update")) { //校验权限
    System.out.println("you can update user");
} else {
    System.out.println("Sorry, you can not update.");
}

// 用户退出，会清除用户状态，身份信息，登录状态信息，权限信息，角色信息，会话信息，全部抹除
currentUser.logout();

// System.exit(0);
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849
```

### 3.4 权限规则

> 最常用的权限标识：【**资源 ：操作**】

```markdown
1> user:query , user:insert , order:delete , menu:show
  	【:】 作为分隔符，分隔资源和操作【资源:操作】
  	【,】 作为分隔，分隔多个权限【权限1,权限2,权限3】
2> user:* , *:query
  	【*】 作为通配符，代表所有操作、资源
  	【user:* 即user的所有操作】
  	【*:query 即所有资源的查询操作】
3> *
  	【代表一切资源的一切权限 = 最高权限】
4> 细节：
	1)【user:*  可以匹配 user:xx, user:xx:xxx】
  	  【*:query 只可以匹配 xx:query,不能匹配 xx:xx:query. 除非 *:*:query】
  	2)【user:update,user:insert】 可以简写为 【"user:update,insert"】 注意：要用""包一下
       [roles]
       manager1=user:query,user:update,user:insert
       manager2="user:query,update,insert" #注意要加引号
       #如上manager1和manager2权限等价
       #subject.isPermittedAll("user:update","user:insert","user:query")
123456789101112131415161718
```

> 实例级权限标识：【**资源 ：操作 ：实例**】，粒度细化到具体某个资源实例

```markdown
1> user:update:1 , user:delete:1
   # 对用户1可以update,对用户1可以delete
2> "user:update,delete:1" #和上面等价
   # subject.isPermittedAll("user:update,delete:1","user:update:1","user:delete:1")
3> user:*:1 , user:update:* , user:*:*
```

## 四、与Web集成

**1.导依赖**

**2.安装，监听ShiroFilter**

**3.配置文件**

> **与web项目集成后，shiro的工作模式如下：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191010075259429.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MDg1OTU0,size_16,color_FFFFFF,t_70)

> **如上：ShiroFilter拦截所有请求，对于请求做访问控制**
>
> **如请求对应的功能是否需要有 认证的身份，是否需要某种角色，是否需要某种权限。**

> **1> 如果没有做 身份认证，则将请求强制跳转到登录页面。**
>
>  **如果没有充分的角色或权限，则将请求跳转到权限不足的页面。**
>
> **2> 如果校验成功，则执行请求的业务逻辑**

### 4.1 pom

```xml
<!-- ============ Servlet ============ -->
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.1</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
<!-- ============== SpringMVC ============== -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>4.1.6.RELEASE</version>
</dependency>

<!-- ============ shiro ============ -->
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-core</artifactId>
    <version>1.4.0</version>
</dependency>
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-web</artifactId>
    <version>1.4.0</version>
</dependency>

<!-- ============ log ============ -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.12</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.7.12</version>
</dependency>
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.16</version>
</dependency>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253
```

### 4.2 代码：Servlet

> **定义一些Servlet 或 springMVC的controller，发送请求，验证shiro的验证**

```java
@Controller
@RequestMapping("/user")
public class ShiroController {
    @RequestMapping("/delete")
    public String deleteUser(){//访问此删除功能时要先经过shiro的安全校验
        System.out.println("delete User");
        return "forward:/xx.jsp";
    }
    @RequestMapping("/update")
    public String updateUser(){//访问此更新功能时要先经过shiro的安全校验
        System.out.println("update User");
        return "forward:/xx.jsp";
    }
    @RequestMapping("/insert")
    public String insertUser(){//访问此增加功能时要先经过shiro的安全校验
        System.out.println("insert User");
        return "forward:/xx.jsp";
    }
    @RequestMapping("/login/page")
    public String login(String username,String password){
    	System.out.println("goto login.jsp");
        return "forward:/login.jsp";
    }
    @RequestMapping("/login/logic")
    public String login(String username,String password){//登录功能不能被shiro校验，否则永不能登录
        try{
            Subject subject = SecurityUtils.getSubject();
            subject.login(new UsernamePasswordToken(username,password));
            String uname = (String)subject.getPrincipal();
            System.out.println("uname:"+uname);
        }catch (AuthenticationException e){
            e.printStackTrace();
            return "redirect:/login.jsp";
        }
        return "forward:/success.jsp";
    }
}
12345678910111213141516171819202122232425262728293031323334353637
```

> **springMVC的相关配置照旧。**

### 4.3 配置

### 4.3.1 web.xml

> **安装 `ShiroFilter！！！！`**

```xml
<!-- 接收所有请求，以通过请求路径 识别是否需要 安全校验，如果需要则触发安全校验
     做访问校验时，会遍历过滤器链。(链中包含shiro.ini中urls内使用的过滤器)
     
     会通过ThreadContext在当前线程中绑定一个subject和SecurityManager，供请求内使用
     可以通过SecurityUtils.getSubject()获得Subject
-->
<filter>
    <filter-name>shiroFilter</filter-name>
    <filter-class>org.apache.shiro.web.servlet.ShiroFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>shiroFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
<!-- 在项目启动时，加载web-info 或 classpath下的 shiro.ini ，并构建WebSecurityManager。
     构建所有配置中使用的过滤器链(anon,authc等)，ShiroFilter会获取此过滤器链
-->
<listener>
    <listener-class>org.apache.shiro.web.env.EnvironmentLoaderListener</listener-class>
</listener>
<!-- 自定义ini文件名称和位置
<context-param>
    <param-name>shiroConfigLocations</param-name>
    <param-value>classpath:shiro9.ini</param-value>
</context-param>-->
<!-- springMVC的配置照旧，此处省略...-->
1234567891011121314151617181920212223242526
```

### 4.3.2 shiro.ini

> **新增两个部分(section), [main] 和 [urls]**

```ini
[users]
zhangsan=123,admin
lisi=456,manager,seller
wangwu=789,clerk

[roles]
admin=*
clerk=user:query,user:detail:query
manager=user:*

[main]
#没有身份认证时，跳转地址
shiro.loginUrl = /user/login/page
#角色或权限校验不通过时，跳转地址
shiro.unauthorizedUrl=/author/error
#登出后的跳转地址,回首页
shiro.redirectUrl=/

[urls]
# 如下格式："访问路径 = 过滤器"
#【1.ant路径：？ *  ** 细节如下】
# /user/login/page , /user/login/logic 是普通路径
# /user/* 代表/user后还有一级任意路径 ： /user/a , /user/b , /user/c , /user/xxxxxxxxxxx
# /user/** 代表/user后还有任意多级任意路径: /user/a , /user/a/b/c , /user/xxxx/xxxxxx/xxxxx
# /user/hello? 代表hello后还有一个任意字符: /user/helloa , /user/hellob , /user/hellox

#【2.过滤器，细节如下】
# anon => 不需要身份认证
# authc => 指定路径的访问，会验证是否已经认证身份，如果没有则会强制转发到 最上面配置的loginUrl上
#         ( ops:登录逻辑本身千万不要被认证拦截，否则无法登录 )
# logout => 访问指定的路径，可以登出,不用定义handler。
# roles["manager","seller"] => 指定路径的访问需要subject有这两个角色
# perms["user:update","user:delete"] => 指定路径的访问需要subject有这两个权限
/user/login/page = anon
/user/login/logic = anon
/user/query = authc
/user/update = authc,roles["manager","seller"]
/user/delete = authc, perms["user:update","user:delete"]
/user/logout = logout
#其余路径都需要身份认证【用此路径需谨慎】
/** = authc
#【3.注意】
# url的匹配，是从上到下匹配，一旦找到可以匹配的则停止，所以，通配范围大的url要往后放，
# 如"/user/delete" 和 "/user/**"
1234567891011121314151617181920212223242526272829303132333435363738394041424344
```

### 4.3.3 其他默认过滤器

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191010075318781.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MDg1OTU0,size_16,color_FFFFFF,t_70)

### 4.4 JSP

> **定义 Controller中和 shiro.ini中需要的jsp**

### 4.5 总结

> **通过ShiroFilter和定义在shiro.ini中的配置信息，即可在项目接收用户访问时，进行身份，角色，权限进行访问控制啦！！！！**

## 五、Shiro标签

> shiro提供了很多标签，用于在jsp中做安全校验。
>
> **完成对页面元素的访问控制**

### 5.1 导入shiro标签库

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="shiro" uri="http://shiro.apache.org/tags"%>
<html>
    ....
</html>
12345
```

### 5.2 身份认证

> **`<shiro:authenticated>` `<shiro:user>` `<shiro:guest>` ** **已登录 已登录或记住我 游客(未登录 且 未记住我) **
>
> **`<shiro:notAuthenticated>` `<shiro:principal>` ** **未登录 获取用户身份信息**

```html
<shiro:authenticated>
    欢迎您，<shiro:principal/>
</shiro:authenticated>

<shiro:user> <!-- 常用，包含已登录 且配合记住我，用户体验好 -->
    欢迎您,<shiro:principal/>
</shiro:user>

<shiro:guest>
    欢迎您，未登录，请<a href="<c:url value="/user/login/page"/>">登录</a>
</shiro:guest>

<shiro:notAuthenticated>
    您尚未登录(记住我也算在未登录中)
</shiro:notAuthenticated>
123456789101112131415
```

### 5.3 角色校验

> **`<shiro:hasAnyRoles name="admin,manager">` `<shiro:hasRole name="admin">`** **是其中任何一种角色 是指定角色**
>
> **`<shiro:lacksRole name="admin">`** **不是指定角色**

```html
<table>
    <tr>
        <td>id</td>
        <td>name</td>
        <td>operation</td>
    </tr>
    <tr>
        <td>001</td>
        <td>张三</td>
        <td>
            <shiro:hasAnyRoles name="admin,manager">
                <a href="#" style="text-decoration:none">详情</a>
            </shiro:hasAnyRoles>
            <shiro:hasRole name="admin">
                <a href="#" style="text-decoration: none">删除</a>
            </shiro:hasRole>
            <shiro:lacksRole name="admin">
                <a href="#" style="text-decoration: none">点击升级</a>
            </shiro:lacksRole>
        </td>
    </tr>
</table>
12345678910111213141516171819202122
```

### 5.4 权限校验

> **`<shiro:hasPermission name="user:delete">`** **有指定权限**
>
> **`<shiro:lacksPermission name="user:delete">`** **缺失指定权限**

```html
...
<td>
    <a href="#" style="text-decoration:none">查看详情</a>
    <shiro:hasPermission name="user:delete">
        <a href="#" style="text-decoration: none">删除</a>
    </shiro:hasPermission>
    <shiro:lacksPermission name="user:delete">
        <a href="#" style="text-decoration: none" >无权删除</a>
    </shiro:lacksPermission>
</td>
...
1234567891011
```

### 5.5 自定义标签

> jsp中允许自定义标签，所以可以根据需求 自定义一些shiro标签。

#### 5.5.1 定义标签类

```java
public class MyAllRoleTag extends RoleTag {
    // jsp中使用：<xxx:xx name="角色参数1,角色参数2,..."/>
    private String name;//存储传入的角色参数
    @Override
    public String getName() {
        return name;
    }
    @Override
    public void setName(String name) {
        this.name = name;
    }

    @Override
    protected boolean showTagBody(String name) {
        System.out.println("验证角色:"+name);
        String[] roles = name.split(",");
        Subject subject = getSubject();
        for(String role:roles) {
            if(!subject.hasRole(role)){
                return false;
            }
        }
        return true;
    }
}
12345678910111213141516171819202122232425
```

#### 5.5.2 定义tld文件

> 要放在`WEB-INF`目录下，名称任意

```xml
<!DOCTYPE taglib PUBLIC "-//Sun Microsystems, Inc.//DTD JSP Tag Library 1.2//EN"
        "http://java.sun.com/dtd/web-jsptaglibrary_1_2.dtd">
<taglib>
    <tlib-version>1.1.2</tlib-version>
    <jsp-version>1.2</jsp-version>
    <short-name>zhj</short-name>
    <uri>http://zhj.apache.org/tags</uri>
    <description>Apache Shiro JSP Tag Library.</description>
    <tag>
        <!-- 标签名 <zhj:hasAllRoles .../> -->
        <name>hasAllRoles</name>
      	<!-- 自定义Tag类路径 -->
        <tag-class>com.zhj.tag.MyAllRoleTag</tag-class>
        <body-content>JSP</body-content>
        <description>Displays body content only if the current Subject (user)
            'has' (implies) all the specified roles
        </description>
        <!-- 自定义Tag中属性名：name
        	 <zhj:hasAllRoles name="role1,role2"/>
        -->
        <attribute>
            <name>name</name>
            <required>true</required>
            <rtexprvalue>true</rtexprvalue>
        </attribute>
    </tag>
</taglib>
123456789101112131415161718192021222324252627
```

#### 5.5.3 使用

```jsp
<%@ taglib prefix="zhj" uri="http://zhj.apache.org/tags" %>
<!-- tld中定义的标签名：hasAllRoles, 向Tag类中传递参数：“seller,manager” -->
<zhj:hasAllRoles name="seller,manager">
    有所有的角色2：manager，seller
</zhj:hasAllRoles>
```

## 六、自定义Realm

## 七 、加密

## 八、Spring集成【重点】

## 九、记住我

## 十、Session管理

## 十一、注解开发