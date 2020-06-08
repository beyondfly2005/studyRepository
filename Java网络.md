# BIO-NIO-AIO-Netty

Netty 对NIO进行了封装

BIO

NIO

AIO 是NIO的2.0版 

JAVA BIO NIO AIO都不好用 所有催生了Netty的

## BiO Blacking IO(Input-Output)

输入输出优化的过程

cpu的速度 是磁盘IO的百万倍

阻塞IO  非阻塞IO  同步 异步

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











