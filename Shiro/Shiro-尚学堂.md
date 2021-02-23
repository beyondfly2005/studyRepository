# Shiro从入门到精通

> 视频地址： https://www.bilibili.com/video/BV1Yb411v7fQ?seid=913766433255455012
>
> 源码地址：
>
> 资料地址：

## 01 Shiro概述

### 什么是Shiro

Apache *Shiro*是一个强大且易用的Java安全框架，执行身份验证、授权、密码和会话管理。使用*Shiro*的易于理解的API,您可以快速、轻松地获得任何应用程序,从最小的移动应用程序到最大的网络和企业应用程序。

官网：http://shiro.apache.org/

### 为什么学习shiro

安全管理框架spring security和shiro

Spring security依赖于spring 学习比较复杂，学习曲线比较高。

shiro比较简单，而且shiro比较独立，既可以在java se中使用，也可以在java EE中使用

并且可以在分布式集群环境中使用。

### Shiro的结构体系

![](https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=3630669614,3502898077&fm=26&gp=0.jpg)

- Authentication认证：验证用户是否合法，也就是登录

- Authorization授权：授予谁具有某些资源的权限

- SessionManagement 会话管理：用户登录后的用哪个好信息通过Session Management

  来管理，不管在什么应用中。

- Cryptography 加密：

  提供了常见的一些加密算法，使得在应用中可以很方便的实现数据安全。并且使用很便捷

- Web Support Web用用程序支持

  shiro可以很方便的集成到web应用程序中

- Caching缓存

  Shiro提供了对缓存的支持，支持多种缓存架构，如：Ehcache，Redis，

- Concurrency并发支持

  支持多线程并发执行

- Testing测试：测试支持用来帮助你未完成单元测试和

- Run AS ：支持一个用户在允许的前提下，使用另外一个身份登录

- Remeber Me 记住我

### Shiro的架构

![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592244111453&di=c4bbdcb88500417a0ad471244e4b90e6&imgtype=0&src=http%3A%2F%2Fattach.dataguru.cn%2Fattachments%2Fforum%2F201403%2F10%2F1435301im3t21cbc0itmbi.png)

Subject 主题 ：身份和凭证

Realms 域



## 02 Shiro.ini文件

## 03 Shiro实现认证

## 04 自定义Realm实现认证

## 05 散列算法+凭证配置

## 06 shiro.ini实现授权

## 07 自定义Realm实现授权

## 08 springmvc+shiro+shiro.ini+ea

## 09 ssm+shiro+Realm实现集成