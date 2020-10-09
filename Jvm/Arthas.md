#### 一、Arthas介绍

Arthas是一款**阿里巴巴开源的性能分析神器Arthas（阿尔萨斯）**

官方网站：

https://alibaba.github.io/arthas/index.html

Arthas能为你做什么事情？



当你遇到以下类似问题而束手无策时，`Arthas`可以帮助你解决：

1. 这个类从哪个 jar 包加载的？为什么会报各种类相关的 Exception？
2. 我改的代码为什么没有执行到？难道是我没 commit？分支搞错了？
3. 遇到问题无法在线上 debug，难道只能通过加日志再重新发布吗？
4. 线上遇到某个用户的数据处理有问题，但线上同样无法 debug，线下无法重现！
5. 是否有一个全局视角来查看系统的运行状况？
6. 有什么办法可以监控到JVM的实时运行状态？
7. 怎么快速定位应用的热点，生成火焰图？

`Arthas`支持JDK 6+，支持Linux/Mac/Windows，采用命令行交互模式，同时提供丰富的 `Tab` 自动补全功能，进一步方便进行问题的定位和诊断。



#### 二、Linux上启动被监控的项目

如果是一个JavaWeb项目，基于tomcat部署，服务端tomcat启动后，通过浏览器访问项目首页，验证项目是否可以正常访问。



#### 三、下载arthas工具

可以直接在Linux上通过命令下载

```bash
wget https://alibaba.github.io/arthas/arthas-boot.jar
```

也可以在浏览器直接访问https://alibaba.github.io/arthas/arthas-boot.jar，等待下载成功后，上传到Linux服务器上。

#### 四、启动arthas工具（需要配置好jdk）

执行命令：

```bash
java -jar arthas-boot.jar
```

执行成功后，arthas提供了一种命令行方式的交互方式，arthas会检测当前服务器上的Java进程，并将进程列表展示出来，用户输入对应的编号（1、2、3、4…）进行选择，然后回车（见红色框，进程[1]就是tomcat进程）。

第一次使用arthas需要自动下载一些依赖包，等待下载完成，就会进入到arthas提供的命令行界面。



#### 五、使用Arthas线上热更新实战

```
# 打开另一个ssh连接 查看java进程
jps
jps -lmv
```

```

启动arthas后 选择需要管理的java进程
* [1]: 7796 org.apache.catalina.startup.Bootstrap
  [2]: 9301 org.apache.catalina.startup.Bootstrap
  [3]: 9701 org.apache.catalina.startup.Bootstrap
  [4]: 17578 org.apache.catalina.startup.Bootstrap

```

选择后，会自动下载一些依赖包，等待下载完成，就会进入到arthas提供的命令行界面

使用如下命令查找 加载器

```bash
sc -d *DockdataController| grep classLoaderHash
classLoaderHash   4d2889ff
```

**再使用redefine命令重新加载新编译好的OAuthClient.class**

```shell
$ redefine -c 452c5c14  /tmp/OAuthClient.class
redefine success, size: 1
```

###### 注意事项

> **不允许新增加field/method**
> **正在跑的函数，没有退出不能生效**









#### 参考资料

> https://www.cnblogs.com/shihaiming/p/11388201.html

> https://www.jianshu.com/p/efa46ccdd7f0