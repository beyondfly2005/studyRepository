# Docker Nexus 私服搭建

> 参考文档

使用docker搭建nexus并配置docker私有仓库
https://www.jianshu.com/p/77af52a75ad8

Docker之Nexus搭建Maven仓库
https://www.cnblogs.com/cgy-home/p/11238380.html

### 什么是Nexus

nexus是一个强大的maven仓库管理器,它极大的简化了本地内部仓库的维护和外部仓库的访问.

nexus是一套开箱即用的系统不需要数据库,它使用文件系统加Lucene来组织数据

nexus使用ExtJS来开发界面,利用Restlet来提供完整的REST APIs,通过IDEA和Eclipse集成使用

nexus支持webDAV与LDAP安全身份认证.

nexus提供了强大的仓库管理功能,构件搜索功能,它基于REST,友好的UI是一个extjs的REST客户端,占用较少的内存,基于简单文件系统而非数据库.



Nexus Repository Manager 仓库管理有2个版本，专业版和oss版，oss版是免费的，专业版是收费的，我们使用oss版。

Nexus  于 2016年发布3.0版本； 较之2.x版本有了较大的改变

maven.aliyun.com/nexus/#welcom 就是一个2.x版本系统

增加 Docker NeGet npm Bower的支持

Maven私服



### Docker安装Nexus

```bash

# http://hub.docker.com docker仓库搜索nexus
# 或是使用命令
$ docker search nexus
# 使用nexus3
$ docker search nexus3
```


虚拟机IP  dhcp自动获取 自动续约 每次启动 ip地址不冲突的情况下，ip不变

```bash
$ docker images
$ docker pull sonatype/nexus3
$ cd /usr/local/docker
$ mkdir nexus
$ cd nexus
$ vim docker-compose.yml
```

```bash
$ #安装安装lsof命令
$ yum install lsof
$ # 查看端口
$ lsof -i:8080
$ lsof -i:8081
```

### Docker-Compose文件

```yml
version: '3.1'
services:
  nexus:
    restart: always
    image: sonatype/nexus3:3.22.1
    container_name: nexus
    ports:
      - 8081:8081
    volumes:
      - /usr/local/docker/nexus/data:/nexus-data
```

### Docker启动

```bash
$ # 启动nexus
$ docker-compose up
#或 加-d 以守护进程模式运行
$ docker-compose up -d 

$ # 查看运行的docker容器
$ docker ps
$ # 停止nexus
$ docker-compose stop nexus
$ # 删除容器
$ docker-compose rm nexus
```

### 安装完成后测试

```bash
# 访问 http://192.168.1.252:8081/
# 没有启动
# 查看日志

$ docker logs 0266268ad5e3  # 容器id
# 日志信息如下
# /sonatype-work/nexus3/log': Permission denied
$ chmod 777 data/
# 重新启动
$ docker-compose up

# 浏览器访问 http://192.168.1.252:8081/ 测试成功
```

