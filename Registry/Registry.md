## Registry (镜像管理平台)

#### 安装 Docker Registry私服

##### 简介



##### 安装

```yaml
version: 3.1
services:
  registry:
  image: registry
  restart: always
  container_name: registry
  ports:
    - 5000:5000
  volumns:
    - /usr/local/docker/registry/data:/var/lib/registry
```



##### 测试

启动成功后需要测试服务器端是否能够正常提供服务，有两种方式测试：

- 浏览器访问

  http://ip:5000/v2/

- 终端访问

```bash
curl http://ip:5000/v2/
```



#### 配置Docker Registry客户端

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

