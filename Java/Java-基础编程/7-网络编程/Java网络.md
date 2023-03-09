# BIO-NIO-AIO-Netty

> 参考文章

> http://www.imooc.com/article/265871
>
> https://blog.csdn.net/m0_38109046/article/details/89449305

## BIO NIO AIO 总结

Java 中的 BIO、NIO和 AIO 理解为是 Java 语言对操作系统的各种 IO 模型的封装。程序员在使用这些 API 的时候，不需要关心操作系统层面的知识，也不需要根据不同操作系统编写不同的代码。只需要使用Java的API就可以了。

在讲 BIO,NIO,AIO 之前先来回顾一下这样几个概念：同步与异步，阻塞与非阻塞。

**同步与异步**

- **同步：** 同步就是发起一个调用后，被调用者未处理完请求之前，调用不返回。
- **异步：** 异步就是发起一个调用后，立刻得到被调用者的回应表示已接收到请求，但是被调用者并没有返回结果，此时我们可以处理其他的请求，被调用者通常依靠事件，回调等机制来通知调用者其返回结果。

同步和异步的区别最大在于异步的话调用者不需要等待处理结果，被调用者会通过回调等机制来通知调用者其返回结果。

**阻塞和非阻塞**

- **阻塞：** 阻塞就是发起一个请求，调用者一直等待请求结果返回，也就是当前线程会被挂起，无法从事其他任务，只有当条件就绪才能继续。
- **非阻塞：** 非阻塞就是发起一个请求，调用者不用一直等着结果返回，可以先去干其他事情。

**那么同步阻塞、同步非阻塞和异步非阻塞又代表什么意思呢？**

举个生活中简单的例子，你妈妈让你烧水，小时候你比较笨啊，在哪里傻等着水开（**同步阻塞**）。等你稍微再长大一点，你知道每次烧水的空隙可以去干点其他事，然后只需要时不时来看看水开了没有（**同步非阻塞**）。后来，你们家用上了水开了会发出声音的壶，这样你就只需要听到响声后就知道水开了，在这期间你可以随便干自己的事情，你需要去倒水了（**异步非阻塞**）。

阻塞非阻塞 在发起请求后，你能不能做其他的事情；

同步阻塞 - 买煎饼/买早餐，只能排队买  等待排队你做不了其他事情



Netty 对NIO进行了封装

### BIO  (Blocking I/O)

同步阻塞I/O模式，数据的读取写入必须阻塞在一个线程内等待其完成。

#### 传统BIO

BIO通信（一请求应答）模型图如下

![](https://img-blog.csdnimg.cn/2019042212100021.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM4MTA5MDQ2,size_16,color_FFFFFF,t_70)

采用 **BIO 通信模型** 的服务端，通常由一个独立的 Acceptor 线程负责监听客户端的连接。我们一般通过在 `while(true)` 循环中服务端会调用 `accept()` 方法等待接收客户端的连接的方式监听请求，请求一旦接收到一个连接请求，就可以建立通信套接字在这个通信套接字上进行读写操作，此时不能再接收其他客户端连接请求，只能等待同当前连接的客户端的操作执行完成， 不过可以通过多线程来支持多个客户端的连接，如上图所示。

如果要让 **BIO 通信模型** 能够同时处理多个客户端请求，就必须使用多线程（主要原因是 `socket.accept()`、 `socket.read()`、 `socket.write()` 涉及的三个主要函数都是同步阻塞的），也就是说它在接收到客户端连接请求之后为每个客户端创建一个新的线程进行链路处理，处理完成之后，通过输出流返回应答给客户端，线程销毁。这就是典型的 **一请求一应答通信模型** 。我们可以设想一下如果这个连接不做任何事情的话就会造成不必要的线程开销，不过可以通过 **线程池机制** 改善，线程池还可以让线程的创建和回收成本相对较低。使用`FixedThreadPool` 可以有效的控制了线程的最大数量，保证了系统有限的资源的控制，实现了N(客户端请求数量):M(处理客户端请求的线程数量)的伪异步I/O模型（N 可以远远大于 M），下面一节"伪异步 BIO"中会详细介绍到。

**我们再设想一下当客户端并发访问量增加后这种模型会出现什么问题？**

在 Java 虚拟机中，线程是宝贵的资源，线程的创建和销毁成本很高，除此之外，线程的切换成本也是很高的。尤其在 Linux 这样的操作系统中，线程本质上就是一个进程，创建和销毁线程都是重量级的系统函数。如果并发访问量增加会导致线程数急剧膨胀可能会导致线程堆栈溢出、创建新线程失败等问题，最终导致进程宕机或者僵死，不能对外提供服务。

NIO

(Input-Output)

输入输出优化的过程

cpu的速度 是磁盘IO的百万倍

阻塞IO  非阻塞IO  同步 异步

NIO

AIO 是NIO的2.0版 

JAVA BIO NIO AIO都不好用 所有催生了Netty的

## 



### 阻塞IO 模型

每线程一客户端

Sever - Client

thows IOException

需要必须关闭 通道 不允许thows IOException

try catch finally 处理

```java
ServerSocket ss = new ServerSocket(); 

ss.bind(new InetSocketAddress('',8888));

while (true){

   Socket s = ss.accept();  //1阻塞方法

   new Thread(() ->

       handle(s);

    }).start();

}

//为什么每一个连接 启动一个线程

static  void handle(Socket s){
    try{
       byte[] bytes = new byte[1024] ;
        int len =s.getInpuutStream().read(bytes);  //2 阻塞方法
        s.getOutputStream().write(bytes,0,len);  //3阻塞io
        s.getOutputStream()
    }
}


//######  BIO Client  ###

Socket s = new Socket 
    

    
```

![image-20200518235353280](C:\Users\beyond\AppData\Roaming\Typora\typora-user-images\image-20200518235353280.png)





阻塞IO 效率低





### NIO （New Non-Blocking 非阻塞）

单线程模型 single Thread

只用一个线程处理客户端的读、写

selector 选择器 大管家

每隔一段时间 轮询需要建立连接的客户端，并坚持已经连接的通道是否需要读写

需要读的 进行读处理，需要写的进行写出来

负责client连接 和 连接的client的读写

````java
ServerSocketChannel ssc = SererSocketChannel.open();
ssc.socket().bind
    ssc.configureBlocking(false);

Selector selector = Selector.open();
ssc.register（selector,SelectionKey.OP_ACCEPT);

while(true){
    selector.select();
    Set<SelectionKey> keys = seletor .selectedKeys();
    Iterator<SelectionKey> it = keys.iterator();
    while(it.hasNext()){
        Selector.
    }
}


handle(SelectionKey key){
    if(key.isAcceptable){
        try{
            ServerSocketChannel  ssc
        } catch(){
            
        }
        
    } else if(key.isReadable()){
        SocketChannel sc = null;
        ByteBuffer buffer = ByteBuffer
    }
    
}

ByteBuffer设计很恶心
flip复位操作
//    
````



### NIO NIO-Reactor 模式

响应式

Selector 只负责连接

读写交给线程池workers来做





阻塞-非阻塞







