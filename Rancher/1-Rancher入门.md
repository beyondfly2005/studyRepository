# Rancher是什么

Rancher是一个开源的企业级容器管理平台。通过Rancher，企业再也不必自己使用一系列的开源软件去从头搭建容器服务平台。Rancher提供了在生产环境中使用的管理Docker和Kubernetes的全栈化容器部署与管理平台。

官网: [https://www.rancher.cn/](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.rancher.cn%2F)

# 为什么需要Rancher

在原来, 如果我们需要做一个分布式集群我们需要学习一全套的框架并编码实现如 服务发现, 负载均衡等逻辑, 给开发者造成很大的负担, 不过好在现在有Docker以及他周边的一些技术能在上层解决这些问题, 而应用该怎么开发就怎么开发.

当你选择使用Docker技术栈的时候, 会发现在生产环境中不光光是 `docker run`就能解决的. 还需要考虑比如多主机之间的组网, 容器缩扩容等问题, 于是你去学习kubernetes, 发现好像有点复杂啊, 有没有更傻瓜化一点的? 那就是rancher了.

# 使用Rancher (1.X)

## rancher-server

rancher-server 主要负责图形化管理主机容器, 并且储存用户的数据(账号, 主机信息, 应用(task)等).

他是一个管理者, 管理工作机应该启动什么容器.

### 启动

启动一个单节点server, 并将数据库数据挂载到宿主机, 保证容器删除后数据还在.



```csharp
sudo docker run -d -v /workspace/rancher/mysql:/var/lib/mysql --restart=unless-stopped -p 8081:8080 rancher/server
```

稍等片刻就能通过访问8081端口进入到Rancher UI

因为rancher前端使用ws和后端通讯, 所以如果使用nginx作为代理访问这个服务器需要这样设置:



```php
server {
        listen 80;
        server_name rancher.bysir.store;

        location / {
                proxy_set_header Host $http_host;
                proxy_pass http://127.0.0.1:8081;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection "upgrade";

        }
}
```

### 配置

#### 添加登陆账号

在"ADMIN->Access Contor"添加一个管理员用于登陆, 我使用的是本地认证方式, 没遇到什么问题就不赘述了.

#### 添加新环境

在rancher-server中默认内置了一个Cattle Template的环境, 我也不知道Cattle是个啥, 我们还是用Kubernetes吧, 眼熟.





点击Add Environment按钮

![img](https:////upload-images.jianshu.io/upload_images/3447621-31b9ea3cb23768ab.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)



这段话我们用中文版本来看:

> Rancher 支持将资源分组归属到多个环境。 每个环境具有自己独立的基础架构资源及服务，并由一个或多个用户、团队或组织所管理。
>  例如，您可以创建独立的“开发”、“测试”及“生产”环境以确保环境之间的安全隔离，将“开发”环境的访问权限赋予全部人员，但限制“生产”环境的访问权限给一个小的团队。

先建一个Test试一试



![img](https:////upload-images.jianshu.io/upload_images/3447621-9c34494085a4b60d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

在这个页面点击添加一个主机



![img](https:////upload-images.jianshu.io/upload_images/3447621-a8a56538af1fe3fe.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

## rancher-agent

rancher-agent 也就是主机, 是用来执行具体工作的机器

按照提示来添加主机:



![img](https:////upload-images.jianshu.io/upload_images/3447621-c3052fc74fbebd53.png?imageMogr2/auto-orient/strip|imageView2/2/w/923/format/webp)

在第四步的输入框中填写上主机的ip地址, 在这里我填写的是`10.117.195.190`, 这个ip使用来给ipsec组网的, 所以需要暴露UDP的500和4500端口.

rancher在这里叫你添加的是公网ip, 但在实际生产环境中, 不可能每个主机都有公网ip并且也不应该使用公网建立网络, 所以我们在这里填写内网(私网)ip地址, 只要保证当你添加多个主机的时候他们之间的UDP500和4500能互相访问.

PS: 可以通过以下代码测试端口连通性:



```ruby
$ nc -u -z -v 10.25.170.125 4500
Connection to 10.25.170.125 4500 port [udp/ipsec-nat-t] succeeded!
```

PPS: 阿里的只需要主机在同一个安全组就能实现内网所有端口互通

复制第五步的代码到主机上执行, 执行之后可以通过以下代码看它的运行情况:



```undefined
docker logs rancher-agent
```

稍等片刻你就能在rancher的web页面"INFRASTRUCTURE->Hosts"下找到你刚刚添加的主机.

这时候能看到这个主机的很多服务(容器)正在启动, 不出意外的话能看到这个样子的主机:



![img](https:////upload-images.jianshu.io/upload_images/3447621-d1ea13af3618e040.png?imageMogr2/auto-orient/strip|imageView2/2/w/258/format/webp)

图太长了, 没截完, 反正全绿就可以了

## 疑难杂症

如果遇到红色无法启动的容器, 首先查看日志, 找找有用信息, 然后尝试以下操作:

- 按照错误日志排错, 通过: 经验(玄学), google, issue
- 手动点击重启这个错误容器
- 看一下列出的我遇到的错误(特别是 **重新部署某个主机**)

### 重新部署某个主机

当我们在测试或者某个主机出现某些难以解决的错误的时候, 会经常使用`重启大法`, 注意在重新将这个主机加入到rancher集群之前需要清理到原来运行的container以及挂载出来的volume, 否则的话, 当再次启动rancher/agent之后你会发现很多服务启动不了, 如etcd, kubernetes; 一般来说, 只需要清理 kubernetes留下来的东西就好了.

一般如下操作

1. `docker volume rm etcd`, 如果提示它被某个容器使用了就停止掉这个容器后再操作
2. `rm -rf /var/etcd/backups`, 删除etcd挂载出来的数据

参考这篇官方文章: [清理主机](https://links.jianshu.com/go?to=https%3A%2F%2Francher.com%2Fdocs%2Francher%2Fv1.6%2Fzh%2Fkubernetes%2Fdeleting%2F%23kubernetes)

### ipsec unhealthy

ipsec会将所有主机组网, 当其中有某个主机连接不上的时候其他ipsec节点也会unhealthy, 这时候就需要检查是那个主机的问题, 看其UDP的500和4500端口是否能与其他主机互相访问.

### ipsec 无法启动

ipsec会向rancher-server机器请求得到其他主机的ip地址以实现组网.

但我通过查看ipsec的错误日志发现这个ipsec容器访问不到rancher-server机器的外网地址, 登陆容器执行命令发现`curl http://www.baidu.com`都报错, 这种情况下... 我选择重启docker(没办法了啊, 如果读者有解决方案感谢告诉我哦).

但请谨慎操作呀 `service docker restart`会重启所有的容器, 这将导致所有服务不可用.

### etcd无法启动

*好像etcd无法启动和ipsec没有正常运行有关, 所以先解决ipsec的问题*

在上面说了记得删除volume etcd

更多的可以参考这篇官方文章: [灾难恢复](https://links.jianshu.com/go?to=https%3A%2F%2Francher.com%2Fdocs%2Francher%2Fv1.6%2Fzh%2Fkubernetes%2Fdisaster-recovery%2F)

### etcd节点无限重启

如果你在"INFRASTRUCTURE->Containers"中看到红色的etcd容器并且在不断重启, 不要惊讶.



![img](https:////upload-images.jianshu.io/upload_images/3447621-d4cf899a3be6ca40.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

在[灾难恢复](https://links.jianshu.com/go?to=https%3A%2F%2Francher.com%2Fdocs%2Francher%2Fv1.6%2Fzh%2Fkubernetes%2Fdisaster-recovery%2F)中说到 **Rancher在三个不同的主机上运行多达三个 etcd 实例**, 猜测Rancher会一直在不同的主机上启动etcd直到满足三个节点, 不过etcd也支持低于三个节点的, 所以正常使用是没问题的. 如果实在看不顺眼就添加3个或以上的主机.

### 运行在Rancher的Nginx反代报错: no resolver defined to resolve xxxx



```cpp
server {
    listen       80;
    server_name  localhost;

    location / {
        proxy_pass http://api;
        proxy_set_header   Authorization "";
    }
}
```

这是一个简单的反代功能, 但在Rancher环境中是无法工作的, nginx会报错: no resolver defined to resolve api.

这是因为在Docker环境中, 需要使用Docker自带的Dns服务器才能找对host对应的ip.

所以需要这样:



```cpp
server {
    listen       80;
    server_name  localhost;
    // for Docker 
    // resolver 127.0.0.11 ipv6=off;
    // for Rancher
    resolver 169.254.169.250 ipv6=off;

    location / {
        proxy_pass http://api;
        proxy_set_header   Authorization "";
    }
}
```

在原生Docker中使用127.0.0.11作为Dns服务器, Rancher中使用169.254.169.250作为Dns服务器, 它们都是固定的, 可以写死.

参考:

- [how to get rancher internal dns server ip?](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Francher%2Francher%2Fissues%2F8148)

更多信息请Google关键字: docker nginx resolve.

### 用nginx反代Rancher中的服务

由于nginx可能会缓存proxy_pass中的域名指向的IP, 如果proxy_pass的域名指向的ip是动态的, 那么可能需要这样写



```csharp
server {
    listen       80;
    server_name  localhost;
    // for Docker 
    // resolver 127.0.0.11 ipv6=off;
    // for Rancher
    resolver 169.254.169.250 ipv6=off;
    # 通过变量设置的proxy_pass不会被缓存
    # 同时必须设置 resolver
    set $api "api";

    location / {
        proxy_pass http://$api;
        proxy_set_header   Authorization "";
    }
}
```

这样的写法并不优雅, 更好的办法是使用动态后端, 可是nginx平民版不支持, 但Tengine可以, 你可以试一试.

### 容器无法联网

最近又在自己家里搭建了一个Rancher平台, 使用的是之前淘汰的笔记本, 刷上Ubuntu 20 Desktop, 又踩上坑了.

用Go写的程序, 在发起http请求时报错: `lookup xxx.org on 169.254.169.250:53: server misbehaving`.

看得出来是dns服务器错误, 169.254.169.250是Rancher自建的DNS服务器, 用来实现服务发现.

打开`network-services/metadata/dns`容器的日志

![img](https:////upload-images.jianshu.io/upload_images/3447621-253d00cc168d1c61.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image.png


 会发现有这样的报错:





```bash
msg="Recurser error: dial udp: address fe80::1%3: too many colons in address fqdn=xxx.org. resolver=fe80::1%3"
```

继续进入到容器中打印`resolv.conf`



```cpp
# cat /etc/resolv.conf
search network-services.rancher.internal metadata.network-services.rancher.internal
nameserver fe80::1%3
```

然后`ping baidu.com`都说`unknown host`,

猜测就是这个奇怪的`fe80::1%3`导致的问题.

本以为找到了错误就好解决这个问题, Google嘛, 但发现怎么也搜不到相关信息.

各种尝试:

- 禁用掉ipv6 [×]
- 禁用掉ipv6之后重装`rancher-agent` [×]
- 指定服务器使用DNS解析: 8.8.8.8 [√]

终于再次打印`resolv.conf`发现nameserver 变为了8.8.8.8



```csharp
# cat /etc/resolv.conf
search network-services.rancher.internal metadata.network-services.rancher.internal
nameserver 8.8.8.8
```

现在也可以其他容器也正常联网了.

猜测`metadata/dns`会自动读取网络配置中的DNS, 但我设置了手动配置IP(固定IP), `metadata/dns`读取DNS的逻辑可能就会有问题.



作者：bysir
链接：https://www.jianshu.com/p/3a492440c89b
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。