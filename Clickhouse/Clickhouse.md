## Clickhouse


> https://mp.weixin.qq.com/s/B2loaFg-FUvwFhx4JJn_VQ
> 中文官网文档
> https://clickhouse.com/docs/zh


### docker-compose 方式部署 Clickhouse
```yaml
version: "3.7"
services:
  clickhouse:
    container_name: clickhouse
    image: yandex/clickhouse-server
    volumes:
      - ./data/config:/var/lib/clickhouse
    ports:
      - "8123:8123"
      - "9000:9000"
      - "9009:9009"
      - "9004:9004"
    ulimits:
      nproc: 65535
      nofile:
        soft: 262144
        hard: 262144
    healthcheck:
      test: ["CMD", "wget", "--spider", "-q", "localhost:8123/ping"]
      interval: 30s
      timeout: 5s
      retries: 3
    deploy:
      resources:
        limits:
          cpus: '4'
          memory: 4096M
        reservations:
          memory: 4096M

  tabixui:
    container_name: tabixui
    image: spoonest/clickhouse-tabix-web-client
    environment:
      - CH_NAME=dev
      - CH_HOST=127.0.0.1:8123
      - CH_LOGIN=default
    ports:
      - "18080:80"
    depends_on:
      - clickhouse
    deploy:
      resources:
        limits:
          cpus: '0.1'
          memory: 128M
        reservations:
          memory: 128M
```