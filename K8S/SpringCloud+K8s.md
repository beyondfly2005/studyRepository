# K8S

> https://www.bilibili.com/video/BV1XJ411D7Qu

## P1

#### 课程介绍



#### 配套资料

##### 问题解决



## P2

构建生产级别的生产系统

本集内容

基于VM安装

## P4

Dockerfile

```dockerfile
FROM centos #制作base
LABEL

RUN
WORDIR
ADD
COPY

ADD


```

```dockerfile
FROM

ENV

EXPOSE 80
EXPOSE 443  # 声明暴露80 443 端口

#构建镜像
docker build  . -t hello-k8s:0.0.1
```

```
docker run -d -p 8081:80 hello-k8s:0.0.1
```

##### #上传到中央仓库

注册账号 或者已经用于docker-hub的账号

```
docker login
输入账号密码
docker tag hello-k8s:0.0.1 你的账号/hello-k8s:0.0.1
docker push 你的账号/hello-k8s:0.0.1

#可以尝试将本地文件删除
docerk rmi 你的账号/hello-k8s:0.0.1
然后从docker-hub 下载

docker pull 你的账号/hello-k8s:0.0.1
```

## P5

docker compose



#安装相关软件

```bash
yum install -y git java maven

#clone 源代码
git clone

#编译源代码
mvn clean install -Dmaven.test.skip=true

在目录中有docker-compose.yml
```



##### docker-compose 相关操作

```
#构建镜像
docker-compose build .

#启动服务
docker-compose up -d

#停止服务
docker-compose stop -rmi -v  # 删除所有镜像、挂载

#查看日志
docker-compose logs -f
```

在一个docker-compose中 容器之间 网络是通的

```
docker exec -it pig-upms sh #进入某个容器
ping git-getway
ping redis
```

## P6、搭建企业镜像私服

基于Harbor

```
# 前提条件
已经安装docker-compose

# 解压harbor离线包
tar -zxvf harbor-offline-installer-v1.9.3.tgz

# 修改harbor.yml
vim harbor.yml
hostname： 192.168.1.111 #不使用localhost 改为ip地址


# 执行安装
sh install.sh
```

需要的端口 80  8080

访问habor

http://192.168.1.111/harbor

用户名 admim 默认密码Habor12345 第一个字母大写



#修改docker 接入私服

```
vim /etc/docker/daemon.json
```

```
"insecure-registries": ["192.168.1.111"]
```

将这段json加入到原json中

```
{
"registry-mirrors":["http://aliyun.docker.io"],
"insecure-registries": ["192.168.1.111"]
}
```

```
systemctl restart docker
```



##### 界面

公开 私有

创建一个公开库 pub或public

推送镜像前 标记镜像

```
docker tag myImage:tag 192.168.1.111/pub/myImage:tag
docker push 192.168.1.111/pub/myImage:tag
```

```
docker tag nginx: 192.168.1.111/pub/myImage:tag
docker push 192.168.1.111/pub/myImage:tag
```

先登录到habor

```
docker login 192.168.1.111
用户名 admin
密码 Habor12345
```

##### 创建一个私有库

```

```

```

```



## P7、Rancher来了理论准备

#### 1、Docker

引擎

#### 2、docker-compose

只能在一台机器上操作docker容器



#### 3、Docker Swrm

docker官方推出的，用来管理多台主机的docker容器，可以帮你启动容器，监控容器状态，如果容器状态不正常会帮你重新启动一个新的容器，同时也提供服务之间的负载均衡



#### 4、Kubernetes

kubernetes的角色定位是合Docker Swarm是一样的 ，负则多主机的容器管理



#### 5、Rancher& Kubernets

Rancher是更上层的管理框架，更像一个微容器云的PAAS管理平台，对k8实现了一些扩展，是k8s的管理工具  管理多个k8s



#### 资源准备

runcher-server

rancher-agent

rancher-agent

Harbor NFS文件服务器 文件挂载在NFS服务上



##### 拓扑





主机准备



```
修改主机名
hostnamectl set-hostname=rancher-agent-152
修改网络
vim /etc/network/

重启网卡
systemctl restart network
```



## P8 Rancher2.3 & Kubernetes 1.16





##### docker 加速器

执行shell命令

```bash
curl -sSL http://get.daocloud.io/daotools/set_mirror.sh |sh -s http://2be16b36.m.daocloud.io
```

```bash
#重启
systemctl restart docker
```

```
docker run -d --start=unless-stopped -p 8080:80 -p 8443:443 -e CATTLE_SYSTEM_CATALOG=bundled -e AUDIT_LEVEL=3 rancher/rancher:v2.3.3
```



构建K8S集群



部署SpringCloud到K8S



部署nginx

自动化构建nginx



##### PVC挂载

添加PVC 

全局 存储 添加PV  多主机读写



## P9、部署SpringCloud到K8S



## P10、部署UI



## P11、HA架构



安装AKE



公钥的分发

```
ssh-copy-id runcher@172.17.0.
```



使用RKE构建K8S集群

rancher-cluster.yml

```

```

执行RKE

```
#在rancher用户下 不能在root用户下
RKE up --config ./rancher
```

## P12、通过Rancher HA Helm部署Rancher

Helm是Kubernetes是Kubernetes的包管理工具，

类似CentOS的yum工具

类似Node的npm工具







