# docker 私服搭建 registy

```bash
# 创建安装目录
cd /usr/local/
mkdir docker/registry

# 编辑docker-compose.yml文件
vim docker-compose.yml
```

```yml
version: '3.1'
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
# 启动docker镜像
$ docker-compose up
$ docker-compose up -d
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
	"insecure-registries":["192.168.1.205:5000"]
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

```
#### 部署Docker Registry WebUI

私服安装成功之后就可以使用docker命令行工具对registry做各种操作了，然而不方便的地方是不能直观的查看registry中俄资源情况，如果可以使用UI工具管理就更快了，这里介绍两个Docker Registry WebUI工具

- docker-registry-frontend
- docker-registry-web

两个工具的功能和解密都差不多，这里以 docker-registry-fontend 为例讲解
```bash
# docker registry 只提供了restFul风格的API

# python web UI页面

```

##### docker-registry-fontend




```bash
# 回到registry目录
cd /usr/local/docker/registry

#停掉之前的registry
docker-compose down

#修改 docker-compose.yml文件
vim docker-compose.yml
```
在services节点 增加如下fontdend 配置

```yml
version: '3.1'
services:
  frontend:
    image: konradkleine/docker-registry-frontend:v2
    ports:
      - 8080:80
    volumes:
      - ./certs/frontend.crt:/etc/apache2/server.crt:ro
      - ./certs/frontend.key:/etc/apache2/server.key:ro
    environment:
      - ENV_DOCKER_REGISTRY_HOST=192.168.1.152
      - ENV_DOCKER_REGISTRY_PORT=5000

```

```bash
# 启动
docker-compose up 
docker-compose up -d

# 查看运行的镜像
docker ps
```

浏览器访问 测试前端webUI

http://ip:8080





## Docker 



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

