> 课程内容 ：JWT 认证原理、流程整合springboot实战应用,前后端分离认证的解决方案
>
> 课程地址：https://www.bilibili.com/video/BV1i54y1m7cP?p=1



# JWT实战教程



## 1、什么是JWT

JSON Web Token （Josn Web 令牌）

官网地址：www.jwt.io/introduction/、

##### 官网翻译

JSON Web Token（JWT）是一个开发标准(rfc7519)的 ,它定义了一种紧凑的、自包含的方式，用于在各方直接以JSON对象安全地传递信息。次信息可以验证和信任，因为他是数字签名的，JWT可以使用秘钥（使用HMAC算法）或使用RSA或ECDSA的公钥/私有对进行签名

##### 通俗解释

JWT是JSON Web Token的检查，也就是通过JSON形式作为Web应用中的令牌，用于在各方之间安全地将信息作为JSON对象传输，在船上过程中还可以完成数据加密、签名等相关处理

## 2、JWT能做什么

##### 1、授权

这是使用JWT的常见方案。一旦用户登录，每个后续请求者将包括JWT，从而允许用户访问该令牌允许的路由、服务和资源。单点登录是当今广泛使用JWT的一项功能，因为他开销很小并且可以在不同额域中轻松使用。

##### 2、信息交换

JSON Web Token 在各方之间安全地传递信息的好方法。因为可以对JWT进行签名（例如 使用公钥/私钥对），属于您可以确保发件人是他们所说的人，此外，由于签名是使用表头和有效负载计算的，因此你可以验证内容是否遭到篡改。

## 3、为什么是JWT

#### 基于传统的Session验证

##### 1、认证方式

我们知道，http协议本身是一种无状态的协议，这就意味着如果用户向我们的应用请求



##### 2、认证流程



##### 3、暴露问题



#### 基于JWT认证





## 5、使用JWT

##### 1、引入依赖

```
<!--引入jwt-->
<dependency>
    <groupId>com.auth0</groupId>
    <artifactId>java-jwt</artifactId>
    <version>3.4.0</version>
</dependency>
```



##### 2、生成token

```
Calendar instance = Calendar.getInstance();
instance.add(Calendar.SECOND,1200);
String token = JWT.create()
        //.withHeader(map)//header
        .withClaim("userId",21)
        .withClaim("userName","张三")
        .withClaim("orgId",1000001012L)
        .withClaim("orgName","安健环部")
        .withClaim("entId",2800000000L)
        .withClaim("entName","金石集团")
        .withExpiresAt(instance.getTime())//指定令牌过期时间
        .sign(Algorithm.HMAC256("!Q@w#E$R"));  //签名
System.out.println(token);
```



##### 3、根据令牌和签名解析数据

```java
JWTVerifier jwtVerifier = JWT.require(Algorithm.HMAC256("!Q@w#E$R")).build();
DecodedJWT verify = jwtVerifier.verify("eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJvcmdOYW1lIjoi5a6J5YGl546v6YOoIiwiZW50TmFtZSI6IumHkeefs-mbhuWboiIsImVudElkIjoyODAwMDAwMDAwLCJ1c2VyTmFtZSI6IuW8oOS4iSIsImV4cCI6MTU5ODcwMDgyNSwidXNlcklkIjoyMSwib3JnSWQiOjEwMDAwMDEwMTJ9.-oZQoWuGh0HHKalIyRUhAVaAPmXytTLp93TVVAdB2OY");
System.out.println(verify.getClaim("userId").asLong());
System.out.println(verify.getClaim("userName").asString());
```



##### 4、常见的异常信息

```properties
SignatureVerificationException 	签名不一致异常
TokenExpiredException 			令牌过期异常
AlgorithmMismatchException		算法不匹配异常
InvalidClaimException			失效的payload异常
```



## 6、封装工具类

```java

public class JwtUtils {

    private static final String SING = "!Q@w#E$R";

    /**
     * 生成token
     */
    public static String getToken(Map<String,String> map){
        Calendar instance = Calendar.getInstance();
        instance.add(Calendar.SECOND,1200);
        JWTCreator.Builder builder = JWT.create();
        map.forEach((k,v)->{
            builder.withClaim(k,v);
        });
        String token = builder.withExpiresAt(instance.getTime())//指定令牌过期时间
                .sign(Algorithm.HMAC256(SING));  //签名
        System.out.println(token);
        return token;
    }

    /**
     * 验证token的合法性
     */
    public static void verify(String token){
        JWTVerifier jwtVerifier = JWT.require(Algorithm.HMAC256(SING)).build();
        DecodedJWT verify = jwtVerifier.verify(token);
    }

    /**
     * 获取token中的信息
     */
    public static DecodedJWT getTokenInfo(String token){
        JWTVerifier jwtVerifier = JWT.require(Algorithm.HMAC256(SING)).build();
        DecodedJWT verify = jwtVerifier.verify(token);
        return verify;
    }
}
```



## 7、整合SpringBoot

```
jwt

mybatis

lombok

druid

mysql


```



application.properties

```


```



token验证 优化

传统web项目  使用拦截器

springcloud 中在网关中验证



token放到request请求头中，不用使用参数传递





##### 拦截器

```java
public class JWTInterceptor implements HandlerInterceptor {

    @Override
    public  boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Map<String,Object> map = new HashMap<>();
        //获取请求头中的令牌
        String toke = request.getHeader("token");
        try{
            JwtUtils.verify(toke);
            return true;
        } catch (SignatureVerificationException e){ //签名不一致异常
            e.printStackTrace();
            map.put("msg","无效的签名！");
        } catch (TokenExpiredException e) {         //令牌过期异常
            e.printStackTrace();
            map.put("msg","token已过期！");
        } catch (AlgorithmMismatchException e) {    // 算法不匹配异常
            e.printStackTrace();
            map.put("msg","token加密算法不一致！");
        } catch (InvalidClaimException e) {         //失效的payload异常)
            return true;
        }
        map.put("state",false);
        //将map转为json jackson
        String json = new ObjectMapper().writeValueAsString(map);
        response.setContentType("application/json;charset=UTF-8");
        response.getWriter().println(json);
        return false;
    }
}
```



##### 配置拦截器

```

```



前端token建议放到localstory 或SessionStory中