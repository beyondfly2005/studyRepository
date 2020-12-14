> 参考视频 https://www.bilibili.com/video/BV1xJ411W7K5?p=3
>
> 参考文档 https://blog.csdn.net/qq_40298902/article/details/106543208



#### Idea与Docker整合 一键部署

##### dea集成Docker实现SpringBoot微服务镜像打包一键部署



org.apache.commons.lang3.StringUtils 替换com.github.pagehelper.StringUtil

```dockerfile
FROM java:8
MAINTANIER "gaolongfei <498601485@qq.com>"
VOLUME /tmp
ADD exam.jar exam.jar
EXPOSE 8080
ENTRYPOINT ["java"."-Djava.security.egd=file:/dev/./urandom","-jar","/exam.jar"]
```



```bash
docker images

docker build -t exam .

vim Dockerfile

docker run -d --name exam-8080 -p 8080:8080 exam

docker run -d --rm --name exam-8080 -p 8080:8080 exam

docker ps

firewall-cmd  --list-portrs

firewall-cmd --reload

docker start exam-8080

firewall-cmd --zone=public --add-port=8081/tcp --permanent

```



#### 1、Docker开启远程访问

```bash
#修改Docker服务文件
vim /usr/lib/systemd/system/docker.service

#修改ExecStart这一行

ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock

将文件中的ExecStart注释 新增如下一行  开放2375端口
#ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock
```

###### 重启docker服务

```bash
#重新加载配置文件
systemctl daemon-reload
#重启服务
systemctl restart docker.service
#查看端口是否开启
netstat -nlpt  #如果找不到netstat命令 可以进行安装 yum install net-tools
#直接使用curl访问 测试师傅生效
curl http://127.0.0.1:2375/info
```

###### 修改防火墙配置 开启2375端口访问

```bash
# 查看已经开放的端口
firewall-cmd --list-ports

# 开启端口
firewall-cmd --zone=public --add-port=2375/tcp --permanent

#重启防火墙
firewall-cmd --reload #重启
firewall systemctl stop firewall.sercie #停止防火墙
firewall systemctl disable firewall.service #禁止firewall开机启动
```

#### 2、Idea安装Idea插件

如果是低版本的idea 2017以下 需要安装docker插件，如果是2018 以后的版本 默认已经安装了docker、插件

打开Idea 从File-Setting-Plugins 搜索 Docker 找到 Docker integration 点击install按钮进行安装

安装完成后 需要重启Idea

#### 3、IDEA配置Docker

配置Docker，连接远程docker服务

从File-Setting-Build,Execution,Deployment->Docker打开配置界面

点击加号+ 在Docker 项 右侧中选择Connect to Docker daemon with **TCP socket**

配置Engin API URL为  tcp://192.168.1.252:2375 远程docker的地址



#### 4、maven的docker插件

docker-maven-plugin

```xml
<!--使用docker-maven-plugin插件-->
<plugin>
    <groupId>com.spotify</groupId>
    <artifactId>docker-maven-plugin</artifactId>
    <version>1.0.0</version>
 
    <!--将插件绑定在某个phase执行-->
    <executions>
        <execution>
            <id>build-image</id>
            <!--将插件绑定在package这个phase上。也就是说，用户只需执行mvn package ，就会自动执行mvn docker:build-->
            <phase>package</phase>
            <goals>
                <goal>build</goal>
            </goals>
        </execution>
    </executions>
 
    <configuration>
        <!--指定生成的镜像名-->
        <imageName>hsz1992/${project.artifactId}</imageName>
        <!--指定标签-->
        <imageTags>
            <imageTag>latest</imageTag>
        </imageTags>
        <!-- 指定 Dockerfile 路径-->
        <dockerDirectory>${project.basedir}/src/main/docker</dockerDirectory>
 
        <!--指定远程 docker api地址-->
        <dockerHost>http://192.168.159.130:2375</dockerHost>
 
        <!-- 这里是复制 jar 包到 docker 容器指定目录配置 -->
        <resources>
            <resource>
                <targetPath>/</targetPath>
                <!--jar 包所在的路径  此处配置的 即对应 target 目录-->
                <directory>${project.build.directory}</directory>
                <!-- 需要包含的 jar包 ，这里对应的是 Dockerfile中添加的文件名　-->
                <include>${project.build.finalName}.jar</include>
            </resource>
        </resources>
    </configuration>
</plugin>
```

#### 5 执行命令项目打包

对项目进行打包，并构建到Docker上

```bash
mvn clean package docker:build
```

#### 6、Idea中创建doker容器 运行springboot项目



#### 7、mavan扩展dokcer配置

绑定Docker命令到Maven的各个阶段

我们可以绑定Docker命令到Maven的各个阶段