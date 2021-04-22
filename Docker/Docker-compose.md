# Docker 三剑客之Docker compose

>视频
https://www.bilibili.com/video/BV15E411F75x?p=556

> docker 三剑客

Docker Compose

Docker Machine

Docker Swarm （已过时）



#### 什么是Docker Compose

Docker Compose是Docker官方编排（Orchestration）项目之一，负载快速部署分布式应用。



#### Docker Compose简介

Compose项目是官方开源项目之一，负责实现对Docker容器集群的的快速编排，跟OpenStack中的Heat十分相似

其代码目前在https://github.com/docker/compose上开源。



Compose定位是：定义和运行多个DOcker容器的应用，其前身是开源项目Fig。

日常工作中，经常会碰到需要多个容器相互配合来完成某项任务的情况，例如要实现一个web项目，除了Web服务容器本身，往往还需要再加上后端的数据库服务容器，甚至还包括负载均衡服务器容器等。

Compose 允许用户通过定义一个单独的docker-compose.yml模板文件（YAML格式）来定义 一组关联的应用容器为一个项目（project）。

Compose有两个重要概念：项目和服务

- 服务（Service） 一个应用容器 实际也可以包含若干相同镜像的容器

- 项目 （Project） 关联的容器组成完整业务单元 多个关联服务

Compose的摩尔恩管理对象是项目，通过子命令对项目中的一组容器进行辩解的生命周期管理。

compose由Python编写 实现上调用Docker服务提供API来对容器进行管理。因此只要所操作的平台支持Docker API就可以在其上利用 Compose来进行编排管理。

简化Docker原生API的操作



#### Docker-Compose安装与卸载



> Docker-Compose安装

docker-compose

> 官网 https://github.com/docker/compose/releases



> 下载地址 

https://github.com/docker/compose/releases/download/1.25.5/docker-compose-Linux-x86_64

> 下载到 /usr/local/bin/docker-copose

> 卸载docker

```bash
--centos系统

--ubantu系统
apt-get autoremove docker-ce
```



> 修改加速器

vim /etc/docker/deamon.json

添加

```json
{"registry-mirrors":["https://reg-mirror.qiniu.com/"]}
```

```bash
#配置完成重启docker
sudo systemctl daemon-reload
sudo systemctl restart docker
```



> 安装docker-compose

```bash
sudo curl -L https://github.com/docker/compose/releases/download/1.25.5/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

如果下载不下来，可以手动下载，然后上传服务器

下载github releases 下载不下来 ，可以复制下载链接到

https://d.serctl.com/ 这个网站 代为下载 速度非常快

```bash
##添加可执行权限
chmod +x docker-compose

##docker-compose是基于phtyon编写的可执行文件
#安装后只要能查看版本就说明安装成功了
docker-compose version

```

#### Docker-Compose 使用

> 使用docker-compose 启动一个tomcat

```bash
使用
cd /usr/local/docker/tomcat/ 
vim docker-compose.yml

```

```yaml
version: "3"
services:
  tomcat:
    restart: always
    image: tomcat
    container_name: tomcat
    ports:
      - 8080:8080
    
  
```

```bash
#启动 docker-compose
docker-compose up

# 查看启动的镜像 (可以打开另一个新ssh连接查看)
docker ps 

# 查看已经终止的容器
docker ps -a 

# 停止/删除一个docker-compose 容器
docker-compose down

# docker-compose 必须在有docker-compose.yml的目录执行

# 在其他目录执行
docker-compose -f  /user/local/docker/tomcat

# 守护态运行
docker-compose up -d

docker ps

# 查看日志
docker-compose logs tomcat
# 一直输出日志 监听
docker-compose logs -f tomcat


```



#### Docker Compose 命令说明

对象与格式

build 构建 dockerfile

down 停止并移除

exec

logs

pull

push

restart

rm



#### Docker Compose 模板文件

dns

dns-serarch

environment

expose

external_links

ports

secrets

#### Docker Compose 实战Tomcat



#### Docker Compose 实战MySQL

```yaml
version: "3.7"
services:
  mysql:
    image: mysql:5.7.27
    container_name: mysql
    ports:
      - 3306:3306
    volumes:
      - /data/mysql/data:/var/lib/mysql
      - /etc/localtime:/etc/localtime
      - /data/mysql/my.cnf:/etc/mysql.conf.d/server.cnf
    environment:
      - MYSQL_ROOT_PASSWORD=dxjc@2021
      - MYSQL_USER=tykj
      - MYSQL_PASSWORD=tykj
      - MYSQL_DATABASE=tykj
   
```



#### Docker Compose 常用命令



#### YAML配置文件语言



#### 部署MyShop项目

```yaml
version: '3'
services:
  web:
    restart: always
    image: tomcat
    container_name: web
    ports:
      - 8080:8080
    volumes:
      - usr/local/docker/myshopt/ROOT:/usr/local/tomcat/webapps/ROOT

  mysql:
    restart: always
    image: mysql:5.7.22
    container_name: mysql
    ports:
      - 3306:3306
    environment:
      TZ:Asia/Shanghai
      MYSQL_ROOT_PASSWORD: 123456
    command:
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_general_ci
      --explicit_defaults_for_timestamp=true
      --lower_case_table_name=1
      --max_allowed_packet=128M
      --sql-mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
    volumes:
      - mysql-data:/var/lib/mysql
volumes:
  mysql-data: /var/lib/docker/volumes/myshop_mysql-data/_data
```

由docker管理的默认数据卷

/var/lib/docker/volumes/myshop_mysql-data/_data







