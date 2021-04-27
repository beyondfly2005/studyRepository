##### 1、创建安装目录

```bash
cd  /usr/local/docker
mkdir redis
cd redis
vim docker-compose.yml
```

yml文件中写入

```yaml
version: '3'
services:
  redis:
    image: redis
    container_name: redis
    hostname: redis
    restart: always
    ports:
      - 6379:6379
    networks:
      - net_db
    volumes:
      - ./data:/data:rw
      - ./conf/redis.conf:/etc/redis/redis.conf:rw
      - ./logs:/logs
    command:
      redis-server /etc/redis/redis.conf --appendonly yes

networks:
  net_db:
    driver: bridge
```

```yaml
version: "3"
# 管理的服务
services:
  redis:
    container_name: my-redis
    # 指定镜像
    image: redis:5
    restart: always
    ports:
      # 端口映射
      - 800:6379
    volumes:
      # 目录映射
      - "${REDIS_DIR}/conf:/usr/local/etc/redis"
      - "${REDIS_DIR}/data:/data"
    command:
      # 执行的命令
      redis-server --port 6379 --requirepass 123456  --appendonly yes
```

保存退出

如需修改密码 找到宿主机redis docker-compose.yml所在的目录

```
cd /usr/local/docker/redis/conf
vim redis.conf
```

代密码的redis容器

```yaml
redis:
  image: redis
  container_name: my_redis
  command: redis-server /usr/local/etc/redis/redis.conf
  ports:
    - "6379:6379"
  volumes:
    - ./data:/data
    - ./redis.conf:/usr/local/etc/redis/redis.conf
```



##### 2、运行容器

```bash
docker-compose up 
docker-compose up -d
```

##### 3、进入容器

```
#方法1
docker-compose exec redis bash
#方法2
docker-compose run redis bash
```

##### 4、查看日志

```
docker-compose  logs -f
```

##### 5、设置为可远程访问

```
redis.conf配置文件：（注释掉bind:127.0.0.1）和修改保护模式为no


```

