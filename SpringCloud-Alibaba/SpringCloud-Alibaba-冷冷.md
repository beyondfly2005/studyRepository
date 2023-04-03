> https://www.bilibili.com/video/BV1yb411b7kP
> https://www.bilibili.com/video/av45084065

### P1、Nacos 服务注册与发现
### P1、Nacos服务注册与发现

Nacos=配置中心+服务注册于发现

默认你可8848

learn 与 study区别


> 官网：
> https://nacos.io/zh-cn/index.html
> 官方文档
> https://nacos.io/zh-cn/docs/quick-start.html

Nacos = Eurake + Spring Cloud Config

中间件方式提供服务

> git下载地址
> https://github.com/alibaba/nacos/releases
> 
##### 目录结构
- 


##### 启动方式
```Bash
cd bind 
start 

```


## P10、Sentinel 限流功能使用

控制台
sentinel-console.jar 默认端口8080

创建SpringBoot项目 接入sentinel

```xml
<dependency>
  <groupId>com.alibaba.cloud</groupId>
  <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>
```

参数配置
```yml
spring:
  cloud:
    sentinel:
      transport:
        dashboard: localhost:8888   #sentinel控制台的请求地址
```

启动应用

刷新sentinel控制台

懒加载，有流量的时候 才会

写一个Controller接口
```java
@RestController
public class DemoController{
    
}
```
重启SpringBootApplication


限流功能
- 默认通过url进行限流
  - 限流是 报错 Block By Centinel
- 通过资源名称进行限流

写一个接口
```Java
@GetMapping
public String resource(){
    return "resource";    
}
```


重写UrlBlockHandler

```Java

```

将自定义的 加载到  需要写一个配置类
```Java
@Configuration
public {
        
}
```

重启后限流规则就没有了


