#### 1、拉取镜像

```bash
docker pull registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
```

#### 2、配置阿里云镜像加速器

```
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://md4nbj2f.mirror.aliyuncs.com"]
}
EOF
```

#### 3、重载配置文件

```bash
launchctl list | grep docker
launchctl stop com.docker.docker.3976
```

#### 4、重启Docker

```
launchctl stop com.docker.docker.39764
```

#### 5、安装docker容器

```
docker run -dp 9090:8080 -p 1521:1521 registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g

#再来一个
docker run -dp 9091:8080 -p 1522:1521 registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
```

#### 6、使用navicat建立连接

```
初始用户名密码：system/helowin；服务名：helowin
```

　　报错：ORA-21561：OID generation failed