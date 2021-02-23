## SSM整合Swagger遇到的坑



#### 1、SSM整合Swagger的两种方式

- 采用自带的Swagger-ui页面  （目前使用）
- 引入外部swagger-ui

参考文档：	

```
https://blog.csdn.net/qq_43196101/article/details/88662238
https://blog.csdn.net/qq_43196101/article/details/88662238
```

文档 方法二种 明确 

```xml
<servlet-mapping>
	<servlet-namespringMVC</servlet-name>
	<url-pattern>/</url-pattern>
</servlet-mapping>	
```

之前配置的 /*.do  暂时没有测试这样会不会有其他问题

另外注释了 srping-mvc.xml中 session拦截器的使用

#### 2、启动报错 　Unable to infer base url. This is common when using dynamic servlet registration or when the API

```
　　Unable to infer base url. This is common when using dynamic servlet registration or when the API is behind an API Gateway.
The base url is the root of where all the swagger resources are served. For e.g. if the api is available at http://example.org/api/v2/api-docs then the base url is http://example.org/api/.
Please enter the location manually；
```

可能的原因

- 启动类或配置类中缺少注解@EnableSwagger2
- 包扫描  Swagger配置类 
- 包扫描 Controller接口路径
- 浏览器兼容性问题
- 清除浏览器缓存
- 编码的问题，所以尽量不要在@Api中加上中文信息

参考文档

```
https://blog.csdn.net/xingxiupaioxue/article/details/104343090
https://www.jianshu.com/p/0c92ec5bb257
https://blog.csdn.net/wangxinxinsj/article/details/81538393
http://www.voidcn.com/article/p-noehsczz-brt.html  浏览器兼容性问题
https://blog.csdn.net/qq_30770095/article/details/80901657
```



#### 3、报错：Can't read swagger JSON from http://localhost:8080/web/v2/api-docs

```
https://blog.csdn.net/loney_wolf/article/details/96874686
https://www.cnblogs.com/zeussbook/p/9468857.html
https://blog.csdn.net/t518vs20s/article/details/80240152
https://blog.csdn.net/VIP_1205169154/article/details/105156979

```

最终有效的方案（也可能是多种原因导致）

```xml
<servlet-mapping>
	<servlet-namespringMVC</servlet-name>
	<url-pattern>/</url-pattern>
</servlet-mapping>	
```

#### 4、/v2/api-docs 无法访问

```
https://blog.csdn.net/xljx_1/article/details/108053388
```

#### 5、一步一步教你SSM整合swagger

```
https://blog.51cto.com/13416247/2308248
```



#### 4、fetching resource list XXX  , Please wait

spring 版本问题 建议升级Spring版本

> 参考文档：https://blog.csdn.net/weixin_41846320/article/details/82963246

#### 5、Swagger不能使用，可能与拦截器相关

```
https://www.cnblogs.com/niugang0920/p/12186446.html
https://blog.csdn.net/qq_39597203/article/details/84937032
```

#### 6、Swagger多分组配置

```
https://blog.csdn.net/dongchan1847/article/details/101849290
https://blog.csdn.net/shipeng22022/article/details/79984139
https://www.iteye.com/blog/tllyf-2432452
```



#### 6、将spring从4.2.9 升级为5.0.1 遇到的问题：Log4jConfigListener找不到类 ，类过期问题

参考文档：

```
https://www.cnblogs.com/super-admin/p/9783112.html
https://blog.csdn.net/hlx20080808/article/details/81062084
https://blog.csdn.net/zhiyuzhe/article/details/78850238
```



#### 5、将spring从4.2.9 升级为5.0.1 遇到的问题：jackson版本

```
com/fasterxml/jackson/databind/exc/InvalidDefinitionException
```

问题分析：

- 找不到fastxml的jar包
- jar包版本冲突

参考文档

```
https://blog.csdn.net/qq_39326472/article/details/104630277
```

解决方法：

jackson-databind 版本冲突，升级为新版本 ，或改为一致的版本，或者 删除自己的引用 ，spring已经依赖了它 ，不用再单独引用

```xml
		<!--        
		<dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.6</version>
         </dependency>-->
```

#### 6、接口同时显示POST、GET、 PUT、DELETE、UPDETE等方法

原因分析：

​	接口中没有指定使用的RequestMethod方法 rest默认都有

解决方案

```java
    @RequestMapping(value = "/listHelpDoc", method = {RequestMethod.POST , RequestMethod.GET} )
    //@RequestMapping(value = "/listHelpDoc", method = RequestMethod.POST)
	//@RequestMapping(value = "/listHelpDoc", method = RequestMethod.GET)
    @ResponseBody
    public ResultModel listHelpDoc(){
    }
```



#### 7、以HttpSession HttpSevletRequest为参数的接口，参数过多

将HttpSession内置的属性，全部加载为参数，导致接口加载速度过慢

```
creationTime
id
lastAccessedTime
maxInactiveInterval
new
servletContext.classLoader
```

解决方法

让Session自动注入，不作为参数使用，或者hidden=false（没有测试成功）

```java
//之前的代码    
	@ResponseBody
    public ResultModel listHelpDoc(HttpSession session){
        try {
            if(session.getAttribute("user") != null) {
            }
        } catch (Exception e) {
        }
    }
```

```java
//之后的代码    

	@Autowired
	private HttpSession session;

	@ResponseBody
    public ResultModel listHelpDoc(){
        try {
            if(session.getAttribute("user") != null) {
            }
        } catch (Exception e) {
        }
    }
```



#### 8、Swagger加载很慢，Controller过多 控制器中的接口过多

解决方法：采用Swagger分组

如 system 一个组 管理 User Menu Role Function Organization等接口

​     report 一个组 管理 X1Report  X2Report  X3Report 等

​     flow 一个组 管理 

​     form 一个组 管理

​     按业务模块进行分组 每次看文档 只能选择一个分组 （一个业务模块），查看分组中的接口 文档

#### 9、Swagger中隐藏请求参数

```
https://segmentfault.com/q/1010000006720980#
```

#### 10、Swagger注解-@ApiModel 和 @ApiModelProperty

```
https://blog.csdn.net/dejunyang/article/details/89527348
```

#### 11、@ApiModelProperty用法

```
https://www.cnblogs.com/huanghuanghui/p/9086860.html
```

#### 12、@ApiImplicitParam注解

```
https://www.cnblogs.com/fengli9998/p/7921601.html
```

#### 13、Swagger使用教程

```
https://blog.csdn.net/pzq915981048/article/details/82864872
https://zhuanlan.zhihu.com/p/92002503
```

#### 14、Swagger绑定Session里的值

```
https://blog.csdn.net/sayyy/article/details/108624137
https://www.cnblogs.com/winclpt/articles/7218086.html
```

#### 15、Swagger常用注解

```
https://www.jianshu.com/p/12f4394462d5
https://blog.csdn.net/wyb880501/article/details/79576784
```

#### 16、Gradle方式 SSM集成Swagger2

```
https://www.jianshu.com/p/a8cbfadf1289
```

#### 18、Swagger-UI github地址

```
https://github.com/swagger-api/swagger-ui
```

