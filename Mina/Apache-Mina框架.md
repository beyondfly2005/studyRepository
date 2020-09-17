> 课程地址 https://www.bilibili.com/video/BV1i54y1m7sx?p=1



## Mina框架



### 课程要求

J2se基础了解，了解吸引栈的一些基础支撑TCP/UDP

了解TCP的 三次握手  四次挥手以及TCP的滑动窗口协议

UDP报文协议是如何封装报文的

从事Java服务端开发

Mina源码和协议栈

JavaEE或web端程序员或零基础Java程序员都可以做Mina开发

课程阶段：入门互的服务器和



### Mina入门级服务端开发

#### Mina概述

Mina是Apache开发的一开源网络通信框架，基于Java NIO来实现开发

#### 依赖的jar包

- commons-logging-log4j-1.0.4.jar
- log4j-1.2.1.2.jar
- mina-cor-2.0.0-M1.jar
- slf4j-api-1.7.2.jar
- slf4j-log12-1.7.2.jar

#### 编码和解码器

应用程序传输java对象或基本数据类，而网络传输的是二进制对象。所以，应用程序需要将数据编码为二进制编码对象



#### Mina服务器端程序

minaserver

```java
public class MinaServer {

    static int PORT = 7080;
    static IoAcceptor accept = null;

    public static void main(String[] args) {
        try {
            accept = new NioSocketAcceptor();
            //设置编码过滤器
            accept.getFilterChain().addLast("codec", new ProtocolCodecFilter(
                    new TextLineCodecFactory(
                            Charset.forName("UTF-8"),
                            LineDelimiter.WINDOWS.getValue(),
                            LineDelimiter.WINDOWS.getValue()
                    )
            ));
            accept.getSessionConfig().setReadBufferSize(1024);
            accept.getSessionConfig().setIdleTime(IdleStatus.BOTH_IDLE, 10);//10ms
            accept.setHandler(new Myhandler());
            accept.bind(new InetSocketAddress(PORT));
            System.out.println("Server " + PORT);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
Handle
```java
public class Myhandler extends IoHandlerAdapter {

    //异常
    @Override
    public void exceptionCaught(IoSession session, Throwable cause) throws Exception {
        System.out.println("exceptionCaught");
    }

    @Override
    public void sessionClosed(IoSession session) throws Exception {
    }

    @Override
    public void sessionCreated(IoSession session) throws Exception {
        System.out.println("sessionCreated");
    }

    @Override
    public void messageSent(IoSession session, Object message) throws Exception {
        System.out.println("messageSent");
    }

    @Override
    public void sessionIdle(IoSession session, IdleStatus status) throws Exception {
        System.out.println("sessionIdle");
    }

    @Override
    public void sessionOpened(IoSession session) throws Exception {
        System.out.println("sessionOpened");
    }

    @Override
    public void messageReceived(IoSession session, Object message) throws Exception {
        Date date = new Date();
        String msg = (String)message;
        System.out.println("服务器接收到的数据："+msg);
        if(msg.equals("exit")){
            session.close();
        }
        session.write(date);
    }
}
```



客户端使用telnet工具连接

```
telnet 127.0.0.1 7080

```

总结：

1、NIOSocket

2、设置编码解密过滤器

3、设置session属性

4、绑定一个端口

### Mina客户端程序

#### 

#### 客户端程序实例

```

```

### Mina 体系结构

##### Mina体系结构

![img](https://upload-images.jianshu.io/upload_images/1684370-c50d29290b03194f.png?imageMogr2/auto-orient/strip|imageView2/2/w/429/format/webp)



##### Mina工作流程图







##### IOService接口

用于描述客户端和服务器端接口，其子类是connector和Aceptor，分别用于描述客户端和服务器端

IOProceser 多线程环境来处理连接请求流程

IOFilter

IOHandler

IOHandlerAcceptor

IOService接口



##### 类图结构

​											IOService	

​					IOConnector 					IOAcceptor

​				NIOSockertConnector 		NIOSocketAcceptor	

总结：IOConnector->IOProcessor->IOFilter->Handler



### Mina长短连接

##### 长连接：

通信双方长期的保持一个连接状态不断开，比如腾讯qq

长连接比较消耗资源

##### 短连接：

通信双方不是保持一个长期的连接张贴，比如http协议，客户端发起http请求，服务器端处理http请求，当服务器处理完后，返回客户端数据后就断开连接，杜宇下次连接请求需要重新发起请求，这种方式使我们长期使用。



长连接改为短连接

```java
public void messageReceived(IoSession session,Object message) throw Exception{
	String msg(String)message;
	System.out.println("服务器端接收到的数据："+msg);
	if(msg.equals("exit")){
		Data data=new Data();
		session.write(date);
	}
}
public void messageSent (IoSession session,Object message) throw Exception{
    System.out.println("messageSent");
}
```

改为以下 短连接

```java
public void messageReceived(IoSession session,Object message) throw Exception{
	String msg(String)message;
	System.out.println("服务器端接收到的数据："+msg);
    /**
	if(msg.equals("exit")){
		Data data=new Data();
		session.write(date);
	}
	**/
}
public void messageSent (IoSession session,Object message) throw Exception{
    System.out.println("messageSent");
    session.close();//关闭连接
}
```



### IOService接口

1、IOService 	实现了对网络通信的客户端和服务器端的之间的抽象，英语描述客户端的子接口IOConnector，用于描述服务器端的子接口IOAcceptor

2、IOService的作用

IOService可以关了我们的网络通信的客户端和服务端，并且管理连接双方的会话session，同样可以添加过滤器

3、IOService 职责

	- 监听器管理
	- IO处理程序
	- IO会话管理
	- 过滤器链管理
	- 统计管理

IOService  AbstractIoService

- 添加默认过滤器链
- 默认处理实现
- 设置IOHandler
- 维护DefaultIOSessionDataStrutureFactory
- 如果默认不提供，则创建一个Excutor
- 添加默认的异常监控器
- 管理Session配置



##### 类图

通过扩展子接口和抽象子类达到扩展的目的

​					IOService          -———————————> abstractIOService

IOAcceptor	IOConnector   ----------------> abstractIOAcceptor     abstract IOConnector  			

##### 相关 API

###### IOServce 常用API  

可以获取常用的过滤器

getFilterChain()  获取过滤器链

setHandler 设置业务Handler

getSessionConfig ()  得到会话配置信息

dispose()  关闭连接



###### IOConnector 常用的API

accept()  用户发起一个连接请求

setConnectTimeout() 连接超时设置



###### IOAcceptor

bind()	 绑定端口

getLoacalAddress()



###### NioSocketAccept API

accept()  j接受一个连接

opern()  打开一个socketchannel

selcet() 获取选择器



##### NioSocketConnector

connect()  用于连接的请求

registry() 注册IO事件



### IOFilter接口

理解过滤器过滤器



IOFilter作用：对于应用程序和网络的传递，就是二进制数据和对象之间的相互转化，用相应的编码和解码器，

这只是过滤器的一种，我们对过滤器还可以做日志， 消息确认。



###### Filter类图

​						IOFilter

IOFilterAdaptor		

##### 常用API



##### 使用IOFilter自定义过滤器

继承IoFilterAdaptor

重写方法messageReceive() 和 messaeSent()方法





### IOSession接口

IOSession主要描述网络通信双方缩减了的练级之间的描述

IOSession作用：可以完成对于连接的一些管理，可以发送或者读取数据，并且可以设置会话的上下文信息

IOSessionConfig 提供连接的配置的描述，比如读取缓存区

IOSessionConfig：设置读取缓冲区的一些信息，读和写的空闲时间



###### 提供的API方法

###### IOSession接口

getAttribute() 根据key获得设置的上下文属性

setAttribute() 设置上下问属性

removeAttribute()  移除上下文属性

write()  发送数据

read()	读取数据



###### IOSessionConfig接口

getBothIdleTime()	获取读写通用的空闲时间

setIdleTime()	设置读写通用的空闲时间

setReadBufferSize()	设置缓冲区大小

setRWriteTime()	设置读写超时时间



### IOProcessor接口

IoProcessor是以NIO为基础实现的多线程的方式来完成我们的读写工作

Processor 是为Filter督学原始数据的多线程环境



配置Processor多线程环境

通过NioSocketAcceptor(int processorCount)	构造函数可以指定多线程的个数

通过NioSocketConnector(int processorCount) 构造函数也可以指定多线程的个数



### IOBuffer接口

目标：IOBuffer



IOBuffer是基于JavaNio中的ByteBuffer做了封装，用户操作缓冲区中的数据，包括基本数据类型，以及字节数组和一些对象，其本质就是一个可动态扩展的byte数组



IOBuffer的所有属性

Capacity：当前缓冲区的大小

Position：当前读写位置，也可以认为下一个可读单一的位置：Position<=Capacity的时候，可以完成数据的读写操作

Limit：下一个不可被读写单元的位置Limit<=Capacity



##### 常用API

allocate 开辟指定大写的缓冲区的空间

setAutoExand 设置是否支持动态的扩展

putShot()

putString()

putInt()

flip() 就是让我们的limit=position,pisition=0

isRemining()	缓冲区中是否有数据 position<=limit=true 否则返回false

remining() 	 limit-position的值

reset()	实现清空数据

Clear()	实现数据的覆盖，position=0 重新开始读取缓冲区数据

### 自定义协议介绍



### 自定义协议数据包分析



自定义的编解码工厂