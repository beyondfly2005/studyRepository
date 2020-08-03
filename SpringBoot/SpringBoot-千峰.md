> 视频  https://www.bilibili.com/video/BV15E411F75x?p=576

#### Spring Boot简史

#### Spring Boot简介

#### Spring Boot优缺点

#### 第一个Spring Boot应用

#### Spring Boot单元测试

#### Spring Boot常用配置

##### 自定义Banner

##### SpringBoot 配置文件

##### SpringBoot Starter POM

##### SpringBoot Log日志配置

logging:
    log

##### 关闭特定的自动配置	

排除配置
在Application中
@SpringBootApplcation(exclude={DataSourceAutoConfiguration.class})	

#### Spring整合Thymetleaf

Thymeleaf是一个模板引擎 可以完全替代JSP，相较于其他末班引擎有如下三个极吸引人的特点
Thymeleaf在有网络和无网络的环境下皆可运行 既可以在有


.jsp
	${user.name}
.html
	<span th:text:"${username}">张三</span>
	th:text="${username}"
有数据无数据的情况下都可以使用

支持html原型,。		忽略未定义的标签属性
html解析时，会忽略非原生的属性

Thymeleaf开箱急用的特性，提供标准和spring标准两种方言
嵌套JSTL OGNL表达式

Thymeleaf提供Spring标准方言和一个与SpringMVC完美集成的
可选模块 可以快速实现表单绑定
属性编辑器 国际化等功能

为什么使用Thymeleaf
jar形式发布模式尽量不要使用jsp
这是因为jsp在内嵌sevlet容器
运行有一些问题(内嵌Tomcat JEtty不支持以Jar形式运行JSP,Undertow不支持JSP)

SpingBoot推荐使用Thymeleaf
SpringBoot提供了大量的末班引擎
Thymeleaf
Beatl
	Html需要渲染的 渲染是需要时间
	Beatl宣称比Thymeleaf快10倍


