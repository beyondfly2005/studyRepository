#### Docker 方式安装Nacos

> 参考文档
> https://www.jianshu.com/p/3d3e17bc629f


#### Docker Compose方式安装Nacos

提供了单机版 集群版 持久化等多个版本的Nacos Docker-compose部署

https://github.com/nacos-group/nacos-docker/tree/master/example 

单机版

https://github.com/nacos-group/nacos-docker/blob/master/example/standalone-derby.yaml

```bash
cd /usr/local/docker/
mkdir nacos-standalone
cd nacos-standalone/
touch docker-compose.yml
vim docker-compose.yml

```

参照 https://github.com/nacos-group/nacos-docker/blob/master/example/standalone-derby.yaml

填入

```yaml
version: "2"
services:
  nacos:
    image: nacos/nacos-server:latest
    container_name: nacos-standalone
    environment:
    - PREFER_HOST_MODE=hostname
    - MODE=standalone
    volumes:
    - ./standalone-logs/:/home/nacos/logs
    - ./init.d/custom.properties:/home/nacos/init.d/custom.properties
    ports:
    - "8848:8848"
  prometheus:
    container_name: prometheus
    image: prom/prometheus:latest
    volumes:
      - ./prometheus/prometheus-standalone.yaml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"
    depends_on:
      - nacos
    restart: on-failure
  grafana:
    container_name: grafana
    image: grafana/grafana:latest
    ports:
      - 3000:3000
    restart: on-failure
```

```
docker-compose up -d
```

```
浏览器访问 
http://192.168.1.252:8848
```



#### Nacos持久化

> https://blog.csdn.net/Fine_Cui/article/details/107573484

将数据存储到mysql