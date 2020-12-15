> 视频资源：https://www.bilibili.com/video/BV14z4y1d7eY
>
> 参考文档：https://blog.csdn.net/qq_38697437/article/details/106129153

## 1、基本概念

### 1.1 什么是认证

进入互联网时代，大家每天都在刷收，常用的软件有微信、支付宝、头条、下面那微信举例说明相关的基本概念，在初次使用微信前需要注册成为微信用户，然后输入账号和密码即可登录微信，输入账号和密码登录微信的过程就是认证。

系统为什么要认证？

认证是为了波爱护系统的隐私数据与资源，用户的合法 方可访问该系统的资源。

**认证：**用户认证就是判断一个用户的身份是否合法的过程，用户去访问系统资源时，系统要求验证用户的身份信息，身份合法才可以继续访问，不合法则拒绝访问。常用的用户身份认证方式有：用户名密码登录，二维码登录，手机短信认证，指纹认证、刷脸认证等方式。

### 1.2 什么是会话

用户认证通过后，为了避免用户的每次操作都进行认证可以将用户的信息保存在会话中，会哈就是系统为了保持当前用户的登录状态所提供的机制，常见的有基于session的方式，基于token方式。

**基于session的认证方式**，如下图：

它的交互流程是：用户认证成功后，在服务端生产用户相关的数据保存在session（当前会话）中， 发给客户端的session_id存放到客户端的cookie中，这样用户客户端请求时，带上session_id就可以验证服务daunt是否存在session数据，以此完成用户的合法校验，当用户





**基于token方式**，如下图：

它的交互流程是，用户认证成功后，服务端生产一个token发给客户端，客户端可以放到cookie或者localStory

### 1.3 什么是授权

### 1.4 RBAC

如何实现授权，



## 2、基于Session的认证方式





## 3、Spring Security快速上手





## 4、Spring Security应用详解

### 4.1 集成SpringBoot

### 4.2 工作原理



## 5、分布式系统认证方案

### 

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
<project>
    <dependency>
	    <groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
	</dependency>

    <dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-openfeign</artifactId>
	</dependency>
	<dependency>
		<groupId>com.netflix.hystrix</groupId>
		<artifactId>hystrix-javanica</artifactId>
	</dependency>
    <dependency>
        <groupId>org.springframework.retry</groupId>
        <artifactId>spring-retry</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
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
        <artifactId>spring-data-commons</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-security</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-oauth2</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-jwt</artifactId>
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
@Configuration
@EnableAuthorizationServer
pubic class AuthorizationServer extends AuthorizationServer{
    //配置客户端详细信息服务
    @Override
    public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
        clients.inMemory()	//是由内存方式
                .withClient("c1")	//client_id
                .secret(new BCryptPasswordEncoder().encode("secret"))
                .resourceIds("res1")
                .authorizedGrantTypes("authorization_code","password","client_credentials","implicit","refresh_token")  //该client允许的授权类型 五种授权类型
                .scopes("all") //允许的授权范围
                .autoApprove(false)  //false 跳转到授权页面
                .redirectUris("http://www.baidu.com");
    }
}
```

##### 6.2.3.3 管理令牌

AuthorizationServerTokenService接口定义了一些操作使得你可以对令牌进行一些必要的管理，令牌可以被用来加载身份信息，里面包含了这个令牌的相关权限。

自己可以创建 AuthorizationServerTokenServices 这个接口的实现，需要继承DefaultTokenServices，里面包含了一些有用实现，你可以使用它来修改令牌的格式和令牌的存储。默认的，当它尝试创建一个令牌的时候，是使用随机值来进行填充的，除了持久化令牌是委托一个 TokenStore 接口来实现以外，这个类几乎帮你做了所有的事情。并且 TokenStore 这个接口有一个默认的实现，它就是 InMemoryTokenStore ，如其命名，所有的令牌是被保存在了内存中。除了使用这个类以外，你还可以使用一些其他的预定义实现，下面有几个版本，它们都实现了TokenStore接口：

- **InMemoryTokenStore：**这个版本的实现是被默认采用的，它可以完美的工作在单服务器上（即访问并发量压力不大的情况下，并且它在失败的时候不会进行备份），大多数的项目都可以使用这个版本的实现来进行尝试，你可以在开发的时候使用它来进行管理，因为不会被保存到磁盘中，所以更易于调试。
- **JdbcTokenStore：**这是一个基于JDBC的实现版本，令牌会被保存进关系型数据库。使用这个版本的实现时，你可以在不同的服务器之间共享令牌信息，使用这个版本的时候请注意把"spring-jdbc"这个依赖加入到你的classpath当中。
- **JwtTokenStore：**这个版本的全称是 JSON Web Token（JWT），它可以把令牌相关的数据进行编码（因此对于后端服务来说，它不需要进行存储，这将是一个重大优势），但是它有一个缺点，那就是撤销一个已经授权令牌将会非常困难，所以它通常用来处理一个生命周期较短的令牌以及撤销刷新令牌（refresh_token）。另外一个缺点就是这个令牌占用的空间会比较大，如果你加入了比较多用户凭证信息。JwtTokenStore 不会保存任何数据，但是它在转换令牌值以及授权信息方面与 DefaultTokenServices 所扮演的角色是一样的。

**如何配置令牌服务**

1、定义TokenConfig

在config包下定义TokenConfig，我暂时使用InMemoryTokenStore，生成一个普通的令牌

```java
@Configuration
public class TokenConfig {

    //令牌存储策略 暂时使用内存方式
    @Bean
    public TokenStore tokenStore(){
        return  new InMemoryTokenStore();
    }
}
```

2、定义AuthorizationServerTokenServices

在AuthorizationServer中定义AuthorizationServerTokenServices

```java

    @Autowired
    private TokenStore tokenStore;

    @Autowired
    private ClientDetailsService clientDetailsService;

    @Bean
    public AuthorizationServerTokenServices tokenServices(){
        DefaultTokenServices services = new DefaultTokenServices();
        services.setClientDetailsService(clientDetailsService);
        services.setSupportRefreshToken(true);//是否产生刷新令牌
        services.setTokenStore(tokenStore);
        services.setAccessTokenValiditySeconds(7200); //令牌默认有效期2小时
        services.setRefreshTokenValiditySeconds(259200); //刷新令牌有效期三天
        return services;
    }
```

##### 6.2.3.4 令牌访问端点配置

AuthorizationServerEndpointsConfigurer这个对象的实例可以完成令牌服务以及令牌endpoint配置

###### 配置授权类型（Grant Types）

AuthorizationServerEndpointsConfigurer通过设定以下属性决定支持的 **授权类型（GrantTypes）**

- **authenticationManager：**认证管理器，当你选择了资源所有者密码（password）授权类型的时候，请设置这个属性注入一个 AuthenticationManager 对象。
- **userDetailsService：**如果你设置了这个属性的话，那说明你有一个自己的 UserDetailsService 接口的实现，或者你可以把这个东西设置到全局域上面去（例如 GlobalAuthenticationManagerConfigurer 这个配置对象），当你设置了这个之后，那么 "refresh_token" 即刷新令牌授权类型模式的流程中就会包含一个检查，用来确保这个账号是否仍然有效，假如说你禁用了这个账户的话。
- **authorizationCodeServices：**这个属性是用来设置授权码服务的（即 AuthorizationCodeServices 的实例对象），主要用于 "authorization_code" 授权码类型模式。
- **implicitGrantService：**这个属性用于设置隐式授权模式，用来管理隐式授权模式的状态。
- **tokenGranter：**当你设置了这个属性（即 TokenGranter 接口实现），那么授权将会交由你来完全掌控，并且会忽略掉上面的这几个属性，这个属性一般是用作拓展用途的，即标准的四种授权模式已经满足不了你的需求的时候，才会考虑使用这个。

###### 配置授权端点的URL（Endpoint URLs）:

AuthorizationServerEndpointsConfigurer这个对象有一个叫做pathMapping()的方法用来配置端点URL链接，它有两个参数：

- 第一个参数：String类型，这个端点URL的默认链接。
- 第二个参数：String类型，你要进行替换的URL链接。

以上的参数都将以 "/" 字符为开始的字符串，框架的默认URL链接如下列表，可以作为这个 pathMapping() 方法的第一个参数：

- /oauth/authorize：授权端点。
- /oauth/token：令牌端点。
- /oauth/confirm_access：用户确认授权提交端点。
- /oauth/error：授权服务错误信息端点。
- /oauth/check_token：用于资源服务访问的令牌解析端点。
- /oauth/token_key：提供公有密匙的端点，如果你使用JWT令牌的话。

需要注意的是授权端点这个URL应该被Spring Security保护起来只供授权用户访问。

在AuthorizationServer配置令牌访问端点

```java
@Autowired
private AuthorizationCodeServices authorizationCodeServices;
    
@Autowired
private AuthenticationManager authenticationManager;

@Override
public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
        endpoints.authenticationManager(authenticationManager) //如果采用密码模式 需要配置此项
                .authorizationCodeServices(authorizationCodeServices) //授权码模式
                .tokenServices(tokenServices()) //令牌管理服务 不管什么模式都需要
                .allowedTokenEndpointRequestMethods(HttpMethod.POST);
}
```

##### 6.2.3.5 令牌端点的安全约束

**AuthorizationServerSecurityConfigurer：**用来配置令牌端点（Token Endpoint）的安全约束，在AuthorizationServer中配置如下

```java
   //4 令牌端点的安全约束
    @Override
    public void configure(AuthorizationServerSecurityConfigurer security){
        security.tokenKeyAccess("permitAll()")	        //(1)
                .checkTokenAccess("permitAll()")		//(2)
                .allowFormAuthenticationForClients();	//(3)
    }
```

（1）tokenkey这个endpoint当使用jwtToken且使用非对称加密时，资源服务用于获取公钥而开放的，这里指这个endpoint完全公开

（2）checkToken这个endpoint完全公开

（3）允许表单验证

**授权服务配置总结：**

授权配置分为三大板块，可以关联记忆。

既然要完成认证，它首先得知道客户端信息从哪儿读取，一次要进行客户端详情配置。

既然要颁发token，那么必须得定义token的相关endpoint，以及tokern如何存取，以及客户端支持哪些类型的token。

既然要爆了除了一些endpoint，那么对这些endpoint可以定义一些安全上的约束等。

##### 6.2.3.6 web安全配置

WebSecurityConfig

```java
@Configuration
@EnableGlobalMethodSecurity(securedEnabled = true,prePostEnabled = true)
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
                .authorizeRequests()
                .antMatchers("/r/**")
                .authenticated()
                .anyRequest().permitAll()
                .and()
                .formLogin()
                .loginPage("/login-view")   //允许登录表单
                .loginProcessingUrl("/login")   //登录页面
                .successForwardUrl("/login-success") //自定义登录成功的页面地址
                .and()
                .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED)
                .and()
                .logout()
                .logoutUrl("/logout")
                .logoutSuccessUrl("/login-view?logout");
    }
}
```

#### 6.2.4 授权码模式

##### 6.2.4.1 授权码模式介绍

授权码模式交互图

![img](https://img-blog.csdnimg.cn/20200515143145439.png)

（1） 资源拥有者打开客户端，客户端要求资源拥有者给予授权，它将浏览器被重定向到授权服务器，重定向时，会附件客户端的身份信息。如：

```
http://localhost:53020/uaa/oauth/authorize?client_id=c1&response_type=code&redirect_url=http://www.baidu.com
```

参数列表如下：

- client_id：客户端准入标识
- response_type：授权码模式固定为`code`
- scope：客户端权限
- redirect_uri：跳转uri，当授权码申请成功后会跳转到此地址，并在后边带上`code`参数（授权码）

（2）浏览器出现向授权服务器授权页面，之后将用户同意授权。

（3）授权服务器将授权码（`AuthorizationCode`）转经浏览器发送给`client`(通过`redirect_uri`)

（4）客户端拿着授权码向授权服务器索要访问`access_token`，请求如下：`http://localhost:3001/oauth/token? client_id=c1&client_secret=secret&grant_type=authorization_code&code=5PgfcD&redirect_uri=http://www.baidu.com`

（5）授权服务器返回令牌(`access_token`)
这种模式是四种模式中最安全的一种模式。一般用于`client`是`Web`服务器端应用或第三方的原生`App`调用资源服务的时候。因为在这种模式中`access_token`不会经过浏览器或移动端的`App`，而是直接从服务端去交换，这样就最大限度的减小了令牌泄漏的风险。

##### 6.2.4.2 测试

使用postman测试

```
http://localhost:53020/uaa/token? client_id=c1&client_secret=secret&grant_type=authorization_code&code=5PgfcD&redirect_uri=http://www.baidu.com`
```



#### 6.2.5 简化模式

##### 6.2.5.1 简化模式介绍

简化模式交互图

![img](https://img-blog.csdnimg.cn/20200515161306814.png)

##### 6.2.5.2 测试



#### 6.2.6 密码模式

##### 6.2.6.1 密码模式介绍

##### 6.2.6.2 测试

#### 6.2.7 客户端模式



### 6.3 JWT