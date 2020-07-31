##### 查找可用docker
```bash
docker search redis
```

##### 拉取镜像
```bash
docker pull redis
docker pull redis:latest
```

##### 查看本地镜像
```bash
docker image
```

##### 运行容器
```bash
docker run -itd --name redis-test -p 6379:6379 redis
--返回id
c312f43c0d1ecf14b2ee510d1d5b5e4ee5847d8acc393453ccb924b25ea3155e
```

##### 查看容器的运行信息
```bash
docker ps 
```

##### 进入容器
```bash
docker exec -it redis-test /bin/bash
```

##### 连接测试使用 redis 服务
```bash
redis-cli
```

##### 修改密码
```bash
docker run -d --name redis -p 6379:6379 --restart=always redis:latest redis-server --appendonly yes --requirepass "mypassword"
docker run -d --name redis -p 6379:6379 redis --requirepass "mypassword"
```

##### 开启持久化，挂载目录
```bash
docker run -d --name redis-server -p 6379:6379 -v /usr/redis/redis.conf:/etc/redis/redis.conf -v /usr/redis/data/:/data redis:latest /etc/redis/redis.conf --appendonly yes --requirepass "redis123"
	
	在/usr/redis新建文件夹，拷贝redis.conf配置文件，建data文件夹保存redis持久化数据
    -v 挂在目录，这里本别挂在了redis.conf文件和data文件夹，
    /etc/redis/redis.conf 关键配置，让redis以指定的配置文件启动，而不是默认无配置启动
    --appendonly yes redis启动后开启数据持久化
```


##### 查看看一下最新前5个的container
```bash
docker ps -n 5 
```

##### 终止运行的容器
```bash
docker stop $CONTAINER_ID
docker stop c312f43c0d1e
```

##### 查看终止状态的容器
```bash
docker ps -a
```

##### 销毁/删除容器
```bash
docker rm c312f43c0d1e -f 
```

##### 重新创建容器
```bash
--开启持久化，挂载目录
docker run -d --name redis-server -p 6379:6379 -v /usr/redis/redis.conf:/etc/redis/redis.conf -v /usr/redis/data/:/data redis:latest /etc/redis/redis.conf --appendonly yes --requirepass "ac2020"
```

##### 进入容器
```bash
docker exec -it redis-server /bin/bash
```

##### 连接测试使用 redis 服务
```bash
redis-cli -a ac2020
redis-cli -h 127.0.0.1 -p 6379 -a ac2020
```

##### 启动redis
```bash
docker start redis-server
```


```bash
linux yum install
docker pull
git clone
mvn install
npm install
```
中央仓库、拉取到本地 安装或启动

nexus管理 bower 、docker、maven、npm、nuget、yum、pypi 