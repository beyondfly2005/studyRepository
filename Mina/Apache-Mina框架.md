> 课程地址 https://www.bilibili.com/video/BV1i54y1m7sx?p=1



## Mina框架



### 零、课程要求

J2se基础了解，了解吸引栈的一些基础支撑TCP/UDP

了解TCP的 三次握手  两次挥手以及互动窗口协议

UDP：如何封装报文



Mina源码和协议栈

课程阶段：入门互的服务器和



### 一、 Mina入门级服务端开发

#### Mina概述

Mina是Apache开发的一开源网络通信框架，基于Java NIO来实现开发

#### 依赖的jar包

- commons-logging-log4j-1.0.4.jar
- log4j-1.2.1.2.jar
- mina-cor-2.0.0-M1.jar
- slf4j-api-1.7.2.jar
- slf4j-log12-1.7.2.jar

#### 编码和解码

应用程序传输java对象或基本数据类，而网络传输的是二进制对象。所以，应用程序需要将数据编码为二进制编码对象



#### 开发实例

minaserver

```java
public class MinaServer{
	static into port=7080;
	IOAcceptor acceptor =null;
	public static void main(String[] argus ){
		accept=new NioSocketAcceptor();
		accept.getFilterChain().addLast("codec",ProtocolCodeFiter(
			new TextLineCodeFactory(
				Charset.forName("UTF-8"),
				LineDelimiter.WINDOWS.getValue().
				LineDelimiter.WINDOWS.getValue()
			)
		));
		accept.getSessionConfig().setReadBufferSize(1024);
		accept.getSessionConfig.setIdleTime(IdleStatus.BOTH_IDLE,10);//10ms
		accept.setHandler(new Myhandler());
		accept.bind(new InetSocketAddress(PORT));
        System.out.println("Server "+PORT);
	}
}

public classMyhandler extends IoHandlerHander(){
    @Overrid
    public void exceptionCaught(IoSession session ,Throw){
        
    }
    //消息发送时
    
    //接收时
    public onResive(){
        
    }
    
    //创建时
    
    //空闲时
    
    //关闭时
        
}
```



使用Cmd工具

```
telnet 127.0.0.1 7080

```

