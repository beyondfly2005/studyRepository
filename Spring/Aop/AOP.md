> 视频地址：  https://www.bilibili.com/video/BV1zt4y1Y7AD?from=search&seid=5751216713042919479

## Spring AOP

## 1、主要内容

- 代理模式
- 静态代理
- 动态代理
- Spring AOP
- Spring AOP的实现
  - 注解方式
  - XML方式

## 2、代理模式

代理模式在Java开发中是一种  比较场景的设计模式。设计目的旨在为服务类与客户类 之间插入其他功能，插入的功能对于调用者是透明的，起到伪装控制的作用。如租房的例子：房客、中介、房东。对应于代理模式中即：客户类、代理类、委托类（被代理类）。

为某一个对象（委托类）提供一个代理（代理类），用来控制对这个对象的访问。委托类和代理类有一个共同的父类或父接口。代理类回调请求做预处理、过滤将请求分配给指定对象。

**生活中常见的代理模式**

​	租房中介、婚庆公司等

**代理模式的两个设计原则:**

1. **代理类 与委托类具有相似的行为（共同）**

   委托类和代理类有一个共同的父类或父接口

2. **代理类增强委托类的行为**

   代理类会对委托类的请求做一些预处理、过滤、增强，将请求分配给指定对象。

