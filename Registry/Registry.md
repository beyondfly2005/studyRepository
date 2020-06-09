## Registry (镜像管理平台)

#### 安装 Docker Registry私服

##### 简介

Registry 是一个Docker 镜像存储和管理平台；Docker Hub是Docker官方的公共镜像管理平台，可以搜索需要的镜像，并且把自己的镜像推送到DockerHub；但有时候我们服务器无法访问互联网，或者不希望自己的镜像放到公网当中，那么可以使用Docker Registry老存储和管理自己的镜像。

##### 安装

使用docker-compose来安装，配置文件如下：

```yaml
version: '3.1'
services:
  registry:
    image: registry
    restart: always
    container_name: registry
    ports:
      - 5000:5000
    volumes:
      - /usr/local/docker/registry/data:/var/lib/registry
```

*启动*

```bash
docker-compose up
docker-compose up -d
```

报错

```bash
Creating network "registry_default" with the default driver
ERROR: Failed to Setup IP tables: Unable to enable SKIP DNAT rule:  (iptables failed: iptables --wait -t nat -I DOCKER -i br-8f1a46b0879a -j RETURN: iptables: No chain/target/match by that name.
```

处理

```
service docker restart
```



##### 测试

启动成功后需要测试服务器端是否能够正常提供服务，有两种方式测试：

- 浏览器访问

  http://ip:5000/v2/

  成功返回 {} 空json

- 终端访问

```bash
curl http://ip:5000/v2/
```



#### 配置Docker Registry客户端

将项目做成镜像上传私服服务端，客户端可以从私服拉取镜像，运行docker镜像，实现一处构建 到处运行

systemd系统（Ubuntu Server 16.04 CentOS7）的linux 需要在/etc/docker/daemon.json(配置加速器的文件)中增加如下内容（如果文件不存在需要新建该文件）

```bash
{
  "registry-mirrors":[
    "https://registry.docker-cn.com"
  ],
  "insecure-registries":[
    "ip:5000"
  ]
}
```

**注意该文件必须符合json规范，否则Docker将不能启动**

之后重启Docker服务

```bash
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

##### 检查配置是否生效

使用 docker info 命令手动检查，如果从配置中看到如下内存，说明配置成功（192.168.0.108 为虚拟机ip）

```bash
$ docker info

Insecure Registries:
  192.168.0.108:5000
  127.0.0.0/8
```

##### 测试镜像上传

```bash
## 拉取一个镜像
docker pull tomcat
docker pull hello-world

## 查看全部镜像
docker images

##标记本地镜像并指向目标仓库（ip:port/image_name:tag 该格式为标记版本号）
docker tag tomcat 192.168.0.108:5000/tomcat

## 提交镜像到仓库
docker push 192.168.0.108：5000/tomcat
```

##### 查看全部镜像

```bash
curl -XGET http://192.168.0.108:5000/v2/_catalog
```

##### 查看置顶镜像

```bash
curl -XGET http://192.168.0.108:5000/v2/tomcat/tags/list
```

##### 测试拉取镜像

- 先删除镜像

``` bash
docker rmi tomcat
docker rmi 192.168.0.108:5000/tomcat
```



#### 部署Docker Registry WebUI

私服安装成功之后就可以使用docker命令行工具对registry做各种操作了，然而不方便的地方是不能直观的查看registry中俄资源情况，如果可以使用UI工具管理就更快了，这里介绍两个Docker Registry WebUI工具

- docker-registry-frontend
- docker-registry-web

两个工具的功能和解密都差不多，这里以 docker-registry-fontend 为例讲解

##### docker-registry-fontend

我们使用docker-compose 来安装和运行docker-registry-fontend，docker-compose.yml配置如下

```yml
version: '3.1'
services:
  fontend:
    image: konradkleine/docker-registry-frontend:v2
    ports:
      - 8080:80
    volumes:
      - ./certs/frontend.crt:/etc/apache2/server.ccrt:ro
      - ./certs/frontend.key:/etc/apache2/server.key:ro
    environment:
      - ENV_DOCKER_REGISTRY_HOOST=192.168.1.152
      - ENV_DOCKER_REGISTRY_PORT=5000

```