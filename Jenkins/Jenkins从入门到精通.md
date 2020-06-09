# Jenkins

Jenkins 和 Hudson

### Jenkins

中文文档 

https://www.jenkins.io/zh/

Jenkins安装方式

- docker
- jar包
- war包
- windows安装
- yum install 安装 （本课程）

版本 ：长期支持版LTS





```bash
# docker daemon 没有启动
# 报错 Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

# 使用如下两个命令启动
systemctl daemon-reload
systemctl restart docker.service

# 查找jenkins
docker search jenkins

# 使用第一个jenlins/jenkins 

mkdir /usr/local/jenkins 
cd /usr/local/jenkins

#新建docker-compose.yml文件
touch docker-compose.yml
vim docker-compose.yml
```
```yaml
jenkins:
        image: jenkins/jenkins:lts
        volumes:
            - /data/jenkins/:/var/jenkins_home
            - /var/run/docker.sock:/var/run/docker.sock
            - /usr/bin/docker:/usr/bin/docker
            - /usr/lib/x86_64-linux-gnu/libltdl.so.7:/usr/lib/x86_64-linux-gnu/libltdl.so.7
        ports:
            - "8088:8080"
        expose:
            - "8080"
            - "50000"
        privileged: true
        user: root
        restart: always
        container_name: jenkins
        environment:
            JAVA_OPTS: '-Djava.util.logging.config.file=/var/jenkins_home/log.properties'
```

```bash
# 创建jenkins数据文件
mkdir -p /data/jenkins
# 设置权限
chown -R 1000:1000 /data/jenkins

# jenkins拥有root权限 否则会报错 权限不够 pormissions

# 启动jenkins

```



Jenkins安装

		- 环境 beyondsoft.com



#### 插件安装

清华大学加速

安装插件

上传插件方式 安装插件 upload hpi 文件

高级 upload Plugin 上传插件方式安装



