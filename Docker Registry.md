# docker 私服搭建 registy

```bash
cd /usr/local/
mkdir docker/registry
vim docker-compose.yml



```

```yml
version: 3.1
services:
  registry:
    image: registry
    restart: always
    container_name: registry
    port:
      - 5000:5000
    volumns:
      - /usr/local/docker/registry/data:var/lib/registry
    
```

```bash
$ docker-compose up
```

> 浏览器打开

ip:5000/v2/

docker pull 从官网下载 docker-hub



### 配置客户端

> 将本地docker镜像 将自己的项目做成docker镜像


```bash
cd /etc/docker/
vim daemon.json

#增加本地registy地址
{

}

```

```json
{
    "registry-mirrors":["https://registry.docker-cn.com"]
}
```

改为

```json
{
    "registry-mirrors":["https://registry.docker-cn.com"],
    "insecure-registries":["192.168.1.205:5000"]
}
```

```bash
#重启docker
systemctl daemon-reload
systemctl restart docker


# 检查配置私服是否成功
docker info

# 下载tomcat
docker pull tomcat

#将本地的tomcat标记为 ip 端口 名称
# 
docker tag tomcat 192.168.1.131:5000/tomcat
docker tag tomcat 192.168.1.131:5000/tomcat:5.5.32

# 将本地的docker 推送到私服
docker push 192.168.1.131:5000/tomcat

http://192.168.1.131:5000/tomcat/v2/_catlog
http://192.168.1.131:5000/tomcat/v2/tags/list
http://192.168.1.131:5000/tomcat/v2/


# docker registry 只提供了restFul风格的API

# python web UI页面

docker-registry-fontend

```

回到 registry


```bash
cd registry

#停掉之前的registry
docker-compose down

#修改 docker-compose.yml文件
vim docker-compose.yml
```
```yml
fontend:
  image: konradkleine/docker-registry-frontend:v2
  ports:
    - 8080:80
  volumes:
    - ./certs/frontend.crt:/etc/apache2/server.ccrt:ro
    - ./certs/frontend.key:/etc/apache2/server.key:ro
  environment:
    - ENV_DOCKER_REGISTRY_HOOST=192.168.1.xx
    - ENV_DOCKER_REGISTRY_PORT=5000

```

```bash
# 启动
docker-compose up -d

```





# Docker 



Docker 一次构建 到处运行

```bash
##上传gitlab

# 复制 gitignore文件

# 提交到本地仓库
git add -A

# 提交到本地
git commit -m '初始化提交'

# 提交到远程
git push


```

```bash
cd /usr/local/docker/myproject
git cone ssh://git@

#免密登录
ssj-keygen -t rsa -C "your@email"

.ssh/


cd /usr/local
git clone ssh://git@

cd myproject

# 安装maven
mvn 

cd maven
tar -xzvf apache-maven-3.5.2
rm -rf apache-maven-3.5.2.tar.gz

# 配置maven环境变量
# 能在任何地方输入mvn 命令
vim /etc/profile
#增加如下配置
export MAVNE_HOME/usr/local/apache-maven-3.5.2
export PATH=$MAVEN_HOME/bin:$PATH:HOME/bin

wq # 保存退出

source /etc/profile

#测试mavn 安装
maven -v

#安装java
$ yum java

vim /etc/profile
export JAVA_HOME=/usr/local/jdk1.8
export

wq

source 

java version
mvn -v


#打包
mvn clean package -Dmaven.text.skip=true

# 自动从私服下载maven依赖
# 默认maven仓库地址在/home/.m2/respository

```



### IDEA 配置git

```
file - setting 检查git路径

git add
git commit

提交时 右侧的 checkTODO 取消勾选

```

