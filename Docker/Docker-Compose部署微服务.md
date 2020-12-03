> https://www.bilibili.com/video/BV1PJ411C7sy?p=2



##### Docker-Compose管理工程结构

```
分为三层
工程Project
服务service
容器container

```

docker-comose.yml常用的命令

###### image

 	指定镜像名称

###### build

​	构建镜像文件DockerFile





##### 用Docker-compose编排SpringCloud微服务



编排微服务

1、



做镜像 编排Dockerfile

将eureka  注册中心地址改为 http://localhost:8090/eureka

修改为 http://eureka:8090/eureka

maven 打包

```bash
/app
makedir user
#上传用户微服务
cd /app
makedir order
#上传订单微服务

#

#生成订单
```

###### 创建Eureka服务的Dockerfile

```dockerfile
From java:8
#将本地文件夹挂载到当前容器
VOLUMN /tmp
# 复制文件到容器
ADD intellsecurity-eureka-server-0.1.0.jar /app.jar
# 声明需要暴露的端口
EXPOSE 8761
# 配置容器启动后执行的命令
ENTRYPOINT ["java","-jar","/app.jar"]
```

###### 创建Order服务的Dockerfile

```dockerfile
From java:8
#将本地文件夹挂载到当前容器
VOLUMN /tmp
# 复制文件到容器
ADD intellsecurity-order-server-0.1.0.jar /app.jar
# 声明需要暴露的端口
EXPOSE 8761
# 配置容器启动后执行的命令
ENTRYPOINT ["java","-jar","/app.jar"]
```

###### 创建User微服务的Dockerfile

```dockerfile
From java:8
#将本地文件夹挂载到当前容器
VOLUMN /tmp
# 复制文件到容器
ADD intellsecurity-user-server-0.1.0.jar /app.jar
# 声明需要暴露的端口
EXPOSE 8761
# 配置容器启动后执行的命令
ENTRYPOINT ["java","-jar","/app.jar"]
```



###### 构建Order 微服务的Docker镜像 

使用dockerfile

```bash
cd /app/order
docker build -t order-service .
docker images
```



###### 构建User微服务的Docker镜像

```bash
cd /app/user
docker build -t user-service .
# 查看镜像是否生成
docker images 
```

###### 构建docker-compose

```bash
cd app
vim docker-compose.yml
```

填入以下内容

```yaml
version: '2'
services:
  eureka:
    image: eureka-server
    ports:
      - "8900:8900"
  user:
    image: user
    port:
      - "8001:8001"
  order:
    image: order
    ports:
      - "8002:8002"
```

