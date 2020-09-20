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

自定义编解码工厂、自定义编码/解码器

自定义编解码工厂 实现 ProtocolCodeFactory这个接口

1、实现ProtocolCodeFactory接口

2、实现自定义的 编码器ProtocalDecoder

3、实现自定义的 解码器ProtocalDecoder接口

4、根据自定义的编解码工厂获得编解码对象



##### 为什么要使用自定义的编码器？

因为我们在工作中 往往不是通过一个字符串就可以传输所有的信息，通常传输的是自定义的协议包，并且在应用层和网络通信中存在对象和二进制流之间的转换关系。



##### 常用的自定义协议方法：

1、定长的方式：如：AA Bb Cc OK NO

2、定界符方式：如：helloworld|wacthmen|...|...|..

  通过特殊的方式来区别消息，这样的方式会出现粘包，半包现象

3、自定义协议包：包头+ 包体

​	包头：通常包含：数据包的版本号，以及整个数据包（包头+包体）长度信息

​	包体：实际数据

​	下一节：为写一个实例做完整的准备工作。



### 自定义协议数据包分析

本节内容：

1、自定义协议包：

​		包头：包头（length,flage）

​		包体：(content) 

2、完成客户端不断发送指定数据的数据包，然后在服务端解析。这个过程中我们要解决半包的问题

下一节：完成代码实例



### 自定义协议数据包代码实现



协议包

```java
public class ProtocalPack {
    private int length;
    private byte flag;
    private String content;

    public int getLength() {
        return length;
    }

    public byte getFlag() {
        return flag;
    }

    public String getContent() {
        return content;
    }

    public ProtocalPack(byte flag,String content){
        this.length=content==null? 0 : content.length() +5;
        //byte 长度为1
        //int length 长度为4
    }

    public String toString(){
        StringBuffer sb =new StringBuffer();
        sb.append("length:").append(length);
        sb.append("flag:").append(flag);
        sb.append("content:").append(content);
        return sb.toString();
    }
}
```



### 自定义协议-编解器

```java
package com.beyondsoft.mina.protocol;

import org.apache.mina.core.buffer.IoBuffer;
import org.apache.mina.core.session.IoSession;
import org.apache.mina.filter.codec.ProtocolEncoderAdapter;
import org.apache.mina.filter.codec.ProtocolEncoderOutput;

import java.nio.charset.Charset;

/**
 * 编码器
 */
public class ProtocolEncoder extends ProtocolEncoderAdapter {

    private final Charset charset;

    public ProtocolEncoder(Charset charset) {
        this.charset = charset;
    }

    @Override
    public void encode(IoSession session, Object object, ProtocolEncoderOutput out) throws Exception {
        ProtocolPack value = (ProtocolPack) object;
        IoBuffer buffer = IoBuffer.allocate(value.getLength());
        buffer.setAutoExpand(true); //自动增长
        buffer.putInt(value.getLength());
        buffer.put(value.getFlag());
        if (value.getContent() != null) {
            buffer.put(value.getContent().getBytes());
        }
        buffer.flip();
        out.write(buffer);//发送出去
    }
}
```



### 自定义协议-解码器

```java
package com.beyondsoft.mina.protocol;

import org.apache.mina.core.buffer.IoBuffer;
import org.apache.mina.core.session.AttributeKey;
import org.apache.mina.core.session.IoSession;
import org.apache.mina.filter.codec.ProtocolDecoderOutput;

import java.nio.charset.Charset;
import java.nio.charset.CharsetDecoder;

/**
 * 解码器
 */
public class ProtocolDecoder implements org.apache.mina.filter.codec.ProtocolDecoder {

    private final AttributeKey CONTEXT = new AttributeKey(this.getClass(), "context");
    private final Charset charset;
    private int maxPackLength = 100;

    public int getMaxPackLength() {
        return maxPackLength;
    }

    public void setMaxPackLength(int maxPackLength) {
        if (maxPackLength < 0) {
            throw new IllegalArgumentException("maxPackLength参数：" + maxPackLength);
        }
        this.maxPackLength = maxPackLength;
    }

    public Context getConText(IoSession session) {
        Context ctx = (Context) session.getAttribute(CONTEXT);
        if (ctx == null) {
            ctx = new Context();
            session.setAttribute(CONTEXT, ctx);
        }
        return ctx;
    }

    public void addpend(IoBuffer in) {

    }

    public Charset getCharset() {
        return charset;
    }

    public ProtocolDecoder() {
        this(Charset.defaultCharset());
    }

    public ProtocolDecoder(Charset charset) {
        this.charset = charset;
    }

    @Override
    public void decode(IoSession session, IoBuffer buffer, ProtocolDecoderOutput out) throws Exception {
        final int packHeadLength=5;
        Context ctx= this.getConText(session);
        ctx.append(buffer);
        IoBuffer buf = ctx.getBuffer();
        buf.flip();
        while(buf.remaining()>=packHeadLength){
            buf.mark();
            int length = buf.getInt();
            byte flag =buf.get();
            if(length<0 || length>maxPackLength){
                buf.reset();
                break;
            } else if(length>packHeadLength && length-packHeadLength<=buf.remaining()){
                int oldLimit = buf.limit();
                buf.limit(buf.position()+length-packHeadLength);
                String content = buf.getString(ctx.getDecoder());
                buf.limit(oldLimit);
                ProtocolPack _package = new ProtocolPack(flag,content);
                out.write(_package);

            } else {
                //半包结构：数据包不完整，只读取了一部分，需要缓存起来下次再读
                buf.clear();
                break;
            }
        }
        if(buf.hasRemaining()){ //是否还有剩余数据
            IoBuffer temp =IoBuffer.allocate((maxPackLength)).setAutoExpand(true);
            temp.put(buf);
            temp.flip();
            buf.reset();
            buf.put(temp);
        } else {
            buf.reset();
        }
    }

    @Override
    public void finishDecode(IoSession session, ProtocolDecoderOutput out) throws Exception {

    }

    @Override
    public void dispose(IoSession session) throws Exception {
        Context ctx = (Context) session.getAttribute(CONTEXT);
        if (ctx != null) {
            session.removeAttribute(CONTEXT);
        }
    }

    private class Context {
        private final CharsetDecoder decoder;
        private IoBuffer buffer;

        private Context() {
            decoder = charset.newDecoder();
            buffer = IoBuffer.allocate(80).setAutoExpand(true);
        }

        public void append(IoBuffer in) {
            this.getBuffer().put(in);
        }

        public void reset() {
            decoder.reset();
        }

        public CharsetDecoder getDecoder() {
            return decoder;
        }

        public IoBuffer getBuffer() {
            return buffer;
        }

        public void setBuffer(IoBuffer buffer) {
            this.buffer = buffer;
        }
    }
}

```



### 自定义协议-服务端实例

```java
package com.beyondsoft.mina.protocol;

import org.apache.mina.core.service.IoAcceptor;
import org.apache.mina.core.service.IoHandlerAdapter;
import org.apache.mina.core.session.IdleStatus;
import org.apache.mina.core.session.IoSession;
import org.apache.mina.filter.codec.ProtocolCodecFactory;
import org.apache.mina.filter.codec.ProtocolCodecFilter;
import org.apache.mina.transport.socket.nio.NioSocketAcceptor;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.charset.Charset;

public class ProtocolServer {

    /** 端口 */
    private static final int PORT = 7080;

    public static void main(String[] args) throws IOException {
        IoAcceptor acceptor = new NioSocketAcceptor();
        acceptor.getFilterChain().addLast("coderc", new ProtocolCodecFilter(new ProtocolFactory(Charset.forName("UTF-8"))));
        acceptor.getSessionConfig().setReadBufferSize(1024);
        acceptor.getSessionConfig().setIdleTime(IdleStatus.BOTH_IDLE,10);
        acceptor.setHandler(new MyHandler());
        acceptor.bind(new InetSocketAddress(PORT));
        System.out.println("server start ......");
    }

    static class MyHandler extends IoHandlerAdapter {
        @Override
        public void exceptionCaught(IoSession session, Throwable cause) throws Exception {
            System.out.println("server -> exceptionCaught");
        }

        @Override
        public void messageReceived(IoSession session, Object message) throws Exception {
            ProtocolPack pack = (ProtocolPack) message;
            System.out.println("服务端接收" + pack);
        }

        @Override
        public void sessionIdle(IoSession session, IdleStatus status) throws Exception {
            System.out.println("server -> session idler");
        }

        @Override
        public void sessionCreated(IoSession session) throws Exception {
            System.out.println("创建连接成功-----");;
        }

        @Override
        public void sessionOpened(IoSession session) throws Exception {
            System.out.println("打开连接成功-----");;
        }
    }
}
```



### 自定义协议-客户端实例

```java
package com.beyondsoft.mina.protocol;

import org.apache.mina.core.future.ConnectFuture;
import org.apache.mina.core.future.IoFuture;
import org.apache.mina.core.future.IoFutureListener;
import org.apache.mina.core.service.IoConnector;
import org.apache.mina.core.service.IoHandlerAdapter;
import org.apache.mina.core.session.IdleStatus;
import org.apache.mina.core.session.IoSession;
import org.apache.mina.filter.codec.ProtocolCodecFilter;
import org.apache.mina.transport.socket.nio.NioSocketConnector;

import java.net.InetSocketAddress;
import java.nio.charset.Charset;

public class ProtocolClient {

    private static final String HOST = "127.0.0.1";
    private static final int PORT = 7080;

    static long conuter = 0;

    final static int fil = 100;

    static long start = 0;

    public static void main(String[] args) {
        start = System.currentTimeMillis();
        IoConnector connector = new NioSocketConnector();
        connector.getFilterChain().addLast("coderc", new ProtocolCodecFilter(new ProtocolFactory(Charset.forName("UTF-8"))));
        connector.getSessionConfig().setReadBufferSize(1024);
        connector.getSessionConfig().setIdleTime(IdleStatus.BOTH_IDLE, 10);
        connector.setHandler(new MyHandler());
        ConnectFuture connectFuture = connector.connect(new InetSocketAddress(HOST, PORT));
        connectFuture.addListener(new IoFutureListener<ConnectFuture>() {
            @Override
            public void operationComplete(ConnectFuture future) {
                if (future.isConnected()) {
                    IoSession session = future.getSession();
                    senddata(session);
                }
            }
        });
    }

    public static void senddata(IoSession session) {
        for (int i = 0; i < fil; i++) {
            String content = "watchmen:" + i;
            ProtocolPack pack = new ProtocolPack((byte) i, content);
            session.write(pack);
            System.out.println("客户端发送数据：" + pack);
        }
    }

    //内部类
    static class MyHandler extends IoHandlerAdapter {
        @Override
        public void messageReceived(IoSession session, Object message) throws Exception {
            ProtocolPack pack = (ProtocolPack) message;
            System.out.println("client->" + pack);
        }

        @Override
        public void sessionIdle(IoSession session, IdleStatus status) throws Exception {
            if (status == IdleStatus.READER_IDLE) {
                session.close(true);
//                session.closeNow();
                System.out.println("客户端连接关闭");
            }
        }
    }
}



```

