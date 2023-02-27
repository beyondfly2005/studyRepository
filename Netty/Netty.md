> 参考文档
>
> https://blog.csdn.net/weixin_33674976/article/details/85951680

### 0、前置知识

netty 官方API： http://netty.io/4.1/api/index.html

netty 中文指南：https://waylau.com/netty-4-user-guide/ 



关于NIO基础的知识：

https://my.oschina.net/andylucc/blog/614295

http://www.cnblogs.com/dolphin0520/p/3919162.html　

http://blog.csdn.net/wuxianglong/article/details/6604817  



### 1、Netty入门

#### 1.1 什么是Netty

Netty是由[JBOSS](http://baike.baidu.com/item/JBOSS)提供的一个[java开源](http://baike.baidu.com/item/java开源)框架。Netty提供异步的、[事件驱动](http://baike.baidu.com/item/事件驱动)的网络应用程序框架和工具，用以快速开发高性能、高可靠性的[网络服务器](http://baike.baidu.com/item/网络服务器)和客户端程序。

netty是封装java socket nio的。 类似的功能是 apache的mina。

相对于Tomcat这种Web Server（顾名思义主要是提供Web协议相关的服务的），Netty是一个Network Server，是处于Web Server更下层的网络框架，也就是说你可以使用Netty模仿Tomcat做一个提供HTTP服务的Web容器。说白了，就是一个好使的处理。Socket的东西。

#### 1.1 搭建开发环境

###### 创建项目添加依赖

```xml
<dependency>
    <groupId>io.netty</groupId>
    <artifactId>netty-all</artifactId>
    <version>5.0.0.Alpha2</version>
</dependency>
```

###### 建立响应规则类

```java
package com.beyondsoft.netty01_discard;

import io.netty.buffer.ByteBuf;
import io.netty.channel.ChannelHandlerAdapter;
import io.netty.channel.ChannelHandlerContext;
import io.netty.util.CharsetUtil;
import io.netty.util.ReferenceCountUtil;

/**
 * 服务端处理通道.这里只是打印一下请求的内容，并不对请求进行任何的响应 DiscardServerHandler 继承自
 * ChannelHandlerAdapter， 这个类实现了ChannelHandler接口， ChannelHandler提供了许多事件处理的接口方法，
 * 然后你可以覆盖这些方法。 现在仅仅只需要继承ChannelHandlerAdapter类而不是你自己去实现接口方法。
 *
 * @author Administrator
 */
public class DiscardServerHandler extends ChannelHandlerAdapter {
    /**
     * 这里我们覆盖了chanelRead()事件处理方法。 每当从客户端收到新的数据时， 这个方法会在收到消息时被调用，
     * 这个例子中，收到的消息的类型是ByteBuf
     * @param ctx 通道处理的上下文信息
     * @param msg 接收的消息
     */
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) {

        try {
            ByteBuf in = (ByteBuf) msg;
            // 打印客户端输入，传输过来的的字符
            System.out.print(in.toString(CharsetUtil.UTF_8));
        } finally {
            /**
             * ByteBuf是一个引用计数对象，这个对象必须显示地调用release()方法来释放。
             * 请记住处理器的职责是释放所有传递到处理器的引用计数对象。
             */
            // 抛弃收到的数据
            ReferenceCountUtil.release(msg);
        }
    }

    /***
     * 这个方法会在发生异常时触发
     *
     * @param ctx
     * @param cause
     */
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        /**
         * exceptionCaught() 事件处理方法是当出现 Throwable 对象才会被调用，即当 Netty 由于 IO
         * 错误或者处理器在处理事件时抛出的异常时。在大部分情况下，捕获的异常应该被记录下来 并且把关联的 channel
         * 给关闭掉。然而这个方法的处理方式会在遇到不同异常的情况下有不 同的实现，比如你可能想在关闭连接之前发送一个错误码的响应消息。
         */
        // 出现异常就关闭
        cause.printStackTrace();
        ctx.close();
    }
}
```

###### 创建响应服务

```java
package com.beyondsoft.netty01_discard;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelOption;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;

/**
 * Copyright: Copyright (c) 2020 Anchuang Network Technology Co., Ltd
 *
 * @ClassName: com.beyondsoft.netty01_discard.DiscardServer
 * @Description: 该类的功能描述
 * @version: v1.0.0
 * @author: gaolongfei
 * 
 * 丢弃任何进入的数据 启动服务端的DiscardServerHandler
 */
public class DiscardServer {
    private int port;

    public DiscardServer(int port) {
        super();
        this.port = port;
    }

    public void run() throws Exception {

        /***
         * NioEventLoopGroup 是用来处理I/O操作的多线程事件循环器，
         * Netty提供了许多不同的EventLoopGroup的实现用来处理不同传输协议。 在这个例子中我们实现了一个服务端的应用，
         * 因此会有2个NioEventLoopGroup会被使用。 第一个经常被叫做‘boss’，用来接收进来的连接。
         * 第二个经常被叫做‘worker’，用来处理已经被接收的连接， 一旦‘boss’接收到连接，就会把连接信息注册到‘worker’上。
         * 如何知道多少个线程已经被使用，如何映射到已经创建的Channels上都需要依赖于EventLoopGroup的实现，
         * 并且可以通过构造函数来配置他们的关系。
         */
        EventLoopGroup bossGroup = new NioEventLoopGroup();
        EventLoopGroup workerGroup = new NioEventLoopGroup();
        System.out.println("准备运行端口：" + port);
        try {
            /**
             * ServerBootstrap 是一个启动NIO服务的辅助启动类 你可以在这个服务中直接使用Channel
             */
            ServerBootstrap b = new ServerBootstrap();
            /**
             * 这一步是必须的，如果没有设置group将会报java.lang.IllegalStateException: group not
             * set异常
             */
            b = b.group(bossGroup, workerGroup);
            /***
             * ServerSocketChannel以NIO的selector为基础进行实现的，用来接收新的连接
             * 这里告诉Channel如何获取新的连接.
             */
            b = b.channel(NioServerSocketChannel.class);
            /***
             * 这里的事件处理类经常会被用来处理一个最近的已经接收的Channel。 ChannelInitializer是一个特殊的处理类，
             * 他的目的是帮助使用者配置一个新的Channel。
             * 也许你想通过增加一些处理类比如NettyServerHandler来配置一个新的Channel
             * 或者其对应的ChannelPipeline来实现你的网络程序。 当你的程序变的复杂时，可能你会增加更多的处理类到pipline上，
             * 然后提取这些匿名类到最顶层的类上。
             */
            b = b.childHandler(new ChannelInitializer<SocketChannel>() { // (4)
                @Override
                public void initChannel(SocketChannel ch) throws Exception {
                    ch.pipeline().addLast(new DiscardServerHandler());// demo1.discard
                    // ch.pipeline().addLast(new
                    // ResponseServerHandler());//demo2.echo
                    // ch.pipeline().addLast(new
                    // TimeServerHandler());//demo3.time
                }
            });
            /***
             * 你可以设置这里指定的通道实现的配置参数。 我们正在写一个TCP/IP的服务端，
             * 因此我们被允许设置socket的参数选项比如tcpNoDelay和keepAlive。
             * 请参考ChannelOption和详细的ChannelConfig实现的接口文档以此可以对ChannelOptions的有一个大概的认识。
             */
            b = b.option(ChannelOption.SO_BACKLOG, 128);
            /***
             * option()是提供给NioServerSocketChannel用来接收进来的连接。
             * childOption()是提供给由父管道ServerChannel接收到的连接，
             * 在这个例子中也是NioServerSocketChannel。
             */
            b = b.childOption(ChannelOption.SO_KEEPALIVE, true);
            /***
             * 绑定端口并启动去接收进来的连接
             */
            ChannelFuture f = b.bind(port).sync();
            /**
             * 这里会一直等待，直到socket被关闭
             */
            f.channel().closeFuture().sync();
        } finally {
            /***
             * 关闭
             */
            workerGroup.shutdownGracefully();
            bossGroup.shutdownGracefully();
        }
    }

    //将规则跑起来
    public static void main(String[] args) throws Exception {
        int port;
        if (args.length > 0) {
            port = Integer.parseInt(args[0]);
        } else {
            port = 8084;
        }
        new DiscardServer(port).run();
        System.out.println("server:run()");
    }
}
```

###### 启动/运行服务器

```
执行DiscardServer类的 main方法 启动服务
```

###### 客户端连接测试

使用telnet 来充当client使用

```bash
#命令行 输入 
telnet 127.0.0.1 8084 
#启动telnet 连接服务器
```

默认的telnet 客户端输入是不显示的，不过输入之后在Idea控制台有输出就表示你这个过程跑通了。

