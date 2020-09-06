> 视频地址 https://www.bilibili.com/video/BV1CJ41157KD?p=26



## Zuul

1、修改启动类 

​	 在启动类上 增加@EnableZuulProxy

2、修改application.properties配置路由规则

​	zuul.routes.${app-name}=:/$