> 参考文档：
>
> https://www.jianshu.com/p/5d47e56f89de
>
> Mina学习（一）：mina实现简单服务端与客户端
> https://www.jianshu.com/p/3593cc6ff45f
>
> Mina学习（二）: mina体系结构简要分析
> https://www.jianshu.com/p/ac99f3317608
> 
> Mina学习（三）：实现简单自定义协议包（报文）
> https://www.jianshu.com/p/a88d78bd3b46
> 
> Mina学习（四）：实现自定义编解码器并解决半包，丢包，粘包问题
> https://www.jianshu.com/u/83cd3678313e
> 






# 1.体系结构分析



### 前言

Apache的MINA框架是一个早年非常流行的NIO框架，它出自于Netty之父Trustin Lee大神之手。虽然目前市场份额已经逐渐被Netty取代了，但是其作为NIO初学者入门学习框架是非常合适的，因为MINA足够的简单，它的实现相对于Netty的难易程度，大概只有Netty的40%左右(个人在对比了MINA和Netty的底层实现得出的结论)；然而其在整体架构上的设计是非常类似的，因此在学习完MINA之后再去看Netty，也会相对简单一些。与此同时，一些老的系统在底层实现上也有很多使用了MINA来进行通信的，如果在接手后不懂其原理，也是很难去维护的。因此个人觉得还是有必要去花些时间去好好研究一下它！

### MINA宏观架构

![img](https:////upload-images.jianshu.io/upload_images/1684370-c50d29290b03194f.png?imageMogr2/auto-orient/strip|imageView2/2/w/429/format/webp)

MINA宏观的体系结构

从宏观上面看，**MINA**作为应用程序与底层网络协议的中间桥梁，它所支持的底层协议包括TCP、UDP、in-VM、 RS-232C串口编程协议等。对于我们开发者而言，无论你是在编写客户端还是服务器端程序，我们都只需在MINA之上设计你的应用本身即可，而无需关注底层网络层协议的复杂处理。

### MINA中的组件

上面我们从宏观的角度看完了MINA的整体结构，下面让我们MINA中的组件做一个剖析。

![img](https://upload-images.jianshu.io/upload_images/1684370-71c7a12bd95e7817.png?imageMogr2/auto-orient/strip|imageView2/2/w/731/format/webp)

**MINA中的组件**

从上面的图中，我们可以看到，从广义的划分方式来说，一个基于**MINA**的应用，无论是服务端还是客户端，它都一共分为三个部分：

- I/O Service：负责端口绑定、接受网络连接或者是主动发起网络连接、网络IO读写、生成对应的IOSession(IOSession是MINA对底层网络连接的一个封装)等功能。
- I/O Filter Chain：数据过滤、数据报文的编解码、黑白名单过滤等等。
- I/O Handler：执行真正的业务逻辑。

所以，如果我们想要创建一个基于**MINA**的网络应用程序，其实只需要3步：

1. 创建I/O Service，通常直接使用MINA内置的IO Service即可。
2. 创建一个Filter Chain,MINA也内置了大量Filter Chain的实现，在某些特殊情况下需要自定义IoFilter，比如实现自定义协议的编解码功能。
3. 创建I/O Handler，编写我们真正的业务逻辑，处理不同的消息。



### MINA服务器端架构

上面我们已经分析了MINA的整体架构，下面对其再细分一下，我们一起来看一下MINA服务器端的架构组成。

![img](https:////upload-images.jianshu.io/upload_images/1684370-a238ed475a835571.png?imageMogr2/auto-orient/strip|imageView2/2/w/478/format/webp)

MINA服务器端架构

细心的同学会发现，上面的图与之前的整体架构图对比起来看，其实真正变化的内容就是从**I/O Service**变成了**I/O Acceptor**。

> MINA对I/O Service做了一个整体的抽象，在服务器端，因为是接收连接，因此是I/O Acceptor;而在客户端，因为是主动发起连接，因此就是I/O Connnector。但是无论IOAcceptor还是IOConnector，它们都继承了IOService这个接口。

![img](https://upload-images.jianshu.io/upload_images/1684370-99f3da22ce966c53.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

IOService的结构图













