### 一、 网络编程基础原理

#### 1  网络编程Socket概念

##### 1.1  什么是Socket

网络上两个程序通过一个双向的通信连接实现的数据的交换，这个连接的一段成为一个socket

​	建立网络通信连接至少要一个端口号，socket本质是编程接口API，多TCP/IP的封装，TCP/IP也要

基于ip+端口，点对点

##### 1.2 Socket的连接步骤

根据连接启动的方式

分为三个步骤：服务器监听 客户端请求 连接确认

入股哦豁数据交换+断开连接 那么一共是五个步骤

- 服务器监听
- 客户端请求
- 连接确认

建立连接：三次握手  

- 客户端发起请求request - ip+port
- 服务器端发送 response ACK确认标记
- 客户端 发送ACK确认标记 连接成功

断开连接：四次挥手

- 客户端请求关闭 request close
- 服务器端确认可以关闭 response ACK
- 服务器端 关闭成功 server closed
- 客户端发起关闭成功 client closed

##### 1.3  Java中的Socket

​	java.net包下的基础类库，其中ServerSocker 和Socket是网络编程的基础类型。ServerSockert是服务器的应用模型，Socket是连接的类型，当连接建立成功后，服务器和客户端都有一个Socket对象示例，可以通过这个Socket对象示例，完成会话的所有操作

​	对于一个完整的网络连接来说，Socket是平等的，没有服务请求客户端分级情况。

#### 2 什么是同步和异步

​		同步和异步是针对应用程序与操作内核的交互而言的，同步是指用户的进程或线程触发IO操作并等待或者轮询的去查看IO操作是否就绪；而异步是指用户进程触发IO操作以后便看是做自己的事情，而当IO操作已经完成的时候回得到 IO完成的通知.



#### 3 什么是阻塞和非阻塞

​		阻塞和非阻塞是针对于进程在访问数据的时候，根据IO操作的就绪状态来采取的不同方式，说白了是一种读取或者写入操作函数的实现方式，阻塞方式下读取或者写入函数将一直等待，而非阻塞方式下，读取或者写入方法会立即返回一个状态值。

ajax是异步阻塞的



#### 4 BIO编程

Blocking-IO 同步阻塞的编程方式

BIO编程方式通常是在JDK1.4版本之前常用的编程方式，编程实现过程为：首先在服务端启动一个ServerSocket来监听网络请求，客户端启动Socket发起网络请求，默认情况下ServerSocket会建立一个线程来处理这个请求，如果服务器端没有线程可用，客户端会阻塞等待或者遭到拒绝。

​		且建立好的连接，在通信过程中，是同步的，在并发处理效率上比较低，大致结构如下

![](https://img-blog.csdnimg.cn/2019042212100021.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM4MTA5MDQ2,size_16,color_FFFFFF,t_70)

​		同步并阻塞，服务器实现模式为一个连接一个线程，即客户端有连接请求时，服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情就会造成不必要的线程开销，当然可以通过线程池机制改进。这种改进方式也成为伪异步。

​		BIO 方式适用于连接数目比较小而且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，但呈现直观简单容易理解。

使用线程池机制改善后的BIO模型图如下：

![](https://img-blog.csdnimg.cn/20190422121019666.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM4MTA5MDQ2,size_16,color_FFFFFF,t_70)

```java
//BIO
public class Server {
    public static void main(String[] args){
        int port = genPort(args);
        ServerSocket server = null;
        try{
            server = new ServerSocket(port);
            System.out.print("Server started!");
            while(true){
                Socket socket = server.accept();
                new Thread(new Handler(socket)).start();
            }
        } catch(Exception e){
            e.printStackTrace();
        } finally{
            if(server!=null){
                try{
                    server.close();
                } catch(IOException e){
                    e.printStackTrace();
                }
                server = null;
            }
        }
    }

    static class Handler implements Runnable{
        Socket socket = null;
        public Handler(Socket socket){
            this.socket = socket;
        }

        @Override
        public void run(){
            BufferedReader reader = null;
            PrintWriter writer = null;
            try{
                reader = new BufferedReader(new InputStreamReader(socket.getInputStream(),"UTF-8"));
                writer = new PrintWriter(new OutputStreamWriter(socket.getOutputStream(),"UTF-8"));
                String readMessage = null;
                while(true){
                    System.out.print("Server reading...");
                    if((readMessage=reader.readLine())==null){
                        break;
                    }
                    System.out.println(readMessage);
                    writer.println("server recive:"+readMessage);
                    writer.flush();
                }
            } catch (Exception e){
                e.printStackTrace();
            } finally{
                if(socket!=null){
                    try{
                        socket.close();
                    } catch(Exception e){
                        e.printStackTrace();
                    }
                    socket = null;
                    if(reader !=null){
                        try{
                            reader.close();
                        } catch(IOException e){
                            e.printStackTrace();
                        }
                        reader = null;
                        if(writer!=null){
                            writer.close();
                        }
                        writer = null;
                    }
                }
            }
        }
    }

    private static int genPort(String[] args){
        if(args.length>0){
            try{
                return Integer.parseInt(args[0]);
            } catch(NumberFormatException e){
                return 9999;
            }
        } else {
            return 9999;
        }
    }
}
```

```java

public class Client {
    public static void main(String[] args){
        String host = null;
        int port =0;
        if(args.length>2){
            host = args[0];
            port = Integer.parseInt(args[1]);
        } else {
            host = "127.0.0.1";
            port=9999;
        }
        Socket socket = null;
        BufferedReader reader = null;
        PrintWriter writer =null;
        Scanner s= new Scanner(System.in);
        try{
            socket = new Socket(host,port);
            String message = null;
            reader = new BufferedReader(
                    new InputStreamReader(socket.getInputStream(),"UTF-8"));
            writer = new PrintWriter(socket.getOutputStream(),true);
            while(true){
                message = s.nextLine();
                if(message.equals("exit")){
                    break;
                }
                writer.println(message);
                writer.flush();
                System.out.println(reader.readLine());
            }

        } catch(Exception e){
            e.printStackTrace();
        } finally{
            if(socket!=null){
                try{
                    socket.close();
                } catch(IOException e){
                    e.printStackTrace();
                }
                socket = null;
                if(reader !=null){
                    try{
                        reader.close();
                    } catch(Exception e){
                        e.printStackTrace();
                    }
                }
                reader = null;
                if(writer !=null){
                    writer.close();
                }
                writer = null;
            }
        }
    }
}
```

伪异步BIO  通过线程池优化BIO

```java

/**
 * 连接池优化的BIO实现
 */
public class Server {

    public static void main(String[] args) {
        int port = genPort(args);
        ServerSocket server =null;
        ExecutorService service = Executors.newFixedThreadPool(50);
        try{
            server = new ServerSocket(port);
            System.out.println("server started!");
            while (true){
                Socket socket = server.accept();
                service.execute(new Handler(socket));
            }
        } catch (Exception e){
            e.printStackTrace();
        } finally {
            if(server!=null){

            }
        }
    }

    static class Handler implements Runnable{
        Socket socket = null;
        public Handler(Socket socket){
            this.socket = socket;
        }

        @Override
        public void run(){
            BufferedReader reader = null;
            PrintWriter writer = null;
            try{
                reader = new BufferedReader(new InputStreamReader(socket.getInputStream(),"UTF-8"));
                writer = new PrintWriter(new OutputStreamWriter(socket.getOutputStream(),"UTF-8"));
                String readMessage = null;
                while(true){
                    System.out.print("Server reading...");
                    if((readMessage=reader.readLine())==null){
                        break;
                    }
                    System.out.println(readMessage);
                    writer.println("server recive:"+readMessage);
                    writer.flush();
                }
            } catch (Exception e){
                e.printStackTrace();
            } finally{
                if(socket!=null){
                    try{
                        socket.close();
                    } catch(Exception e){
                        e.printStackTrace();
                    }
                    socket = null;
                    if(reader !=null){
                        try{
                            reader.close();
                        } catch(IOException e){
                            e.printStackTrace();
                        }
                        reader = null;
                        if(writer!=null){
                            writer.close();
                        }
                        writer = null;
                    }
                }
            }
        }
    }

    private static int genPort(String[] args){
        if(args.length>0){
            try{
                return Integer.parseInt(args[0]);
            } catch(NumberFormatException e){
                return 9999;
            }
        } else {
            return 9999;
        }
    }
}
```

#### 5 NIO编程

​		Unblocking IO(New-IO) 同步非阻塞的编程方式。

​		NIO本身是基于事件模型驱动思想来完成的，其主要想解决的是BIO的大并发问题，NIO基于Reactor，当socke有流可读或者可写socket时，操作系统会响应的通知应用程序进行处理，应用在将流读取到缓冲区或者写入操作系统，也就是说，这个时候，已经不是一个连接就要对应一个处理线程了，而是有效的请求，对应一个线程，当连接没有数据时，是没有工作线程来处理的。

​		NIO的最重要的地方是当一个连接创建后，不需要对应一个线程，这个连接会被注册地多路复用器上面，所以所有的连接只需要一个线程就可以搞定，当这个线程中的多路复用器进行轮询的时候，发现连接上有请求的话，才开启也线程进行处理，也就是一个请求一个线程模式。

​		在NIO的处理方式中，当一个请求来的话，开启线程进行处理，可能会等待后端英勇的资源（JDBC连接等），其实这个线程就被阻塞了，当并发量上来的话还是会有BIO一样的问题。



状态编码

- OP_ACCEPT  连接成功的标记位
- OP_READ 可以读取的标记
- OP_WRITE 可以写入的标记位
- OP_CONNECT 连接成功的标记



#### 6 AIO编程

​		Asynchronous IO 异步非阻塞的编程方式

​		与NIO不同，当进行读写操作时，执行直接调用API的read或write方法即可。这两种方法均为异步的，对于读操作而言，当有流可读取时，操作系统会将可读的流传如read方法的缓冲区，并通知应用程序；对于操作而言，当操作系统将write方法传递的流写入完毕时，操作系统主动通知应用程序，即可理解为read write 方法是异步的，完成后会主动调用回调函数，在JDK1.7中，这部分内容被称为NIO.2 主要在java.nio.channels包下增加了下面四个异步通道

​	AsynchronousSocketChannel

​	AsynchronousServerSocket

​	AsynchronousFileChannel

​	AsynchronousDatagramChannel

​	异步非阻塞，服务器实现模式为一个有效请求一个线程，客户端的IO请求都是由OS

### 二、Netty

​	1、简介

​	2、Netty架构

​	3、基础程序演示

​	4、流数据的传输处理