> 视频

https://www.bilibili.com/video/BV15E411F75x?p=569

Docker Deploy服务器- Registry的客户端上

```bash
# 检查git是否安装
git version 

# 生成秘钥
ssh-keygen -t -C 'yourmail@qq.com'

cd /root.ssh/
cat id_rsa.pub
或
vim id_rsa.pub

复制到gitlab settiings  SSH秘钥

回到 Registry-Deploy
cd /usr/local/docker
git clone ssh://git@192.168.1.252:2222/

# maven 打包
mvn package

# 安装maven
cd /usr/local/
cp apache-maven.3.5.tar.gz /usr/local/maven

# 配置环境变量
vim /etc/profile

export MAVEN_HOME=/usr/local/apache-maven-3.5.3
export PATH=$MAVEN_HOME/bin:$PATH:$HOME/bin

source /etc/profile

# 安装java
cd /usr/local/java
cp jdk-8u152-linux-x64.tar.gz /usr/local/java/
tar -zxvf jdk-8u152-linux-x64.tar.gz

# 配置java环境变量
vim /etc/profile

ln -s /usr/local/jdk-8u152-linux-x64 /usr/local/java
export JAVA_HOME=/usr/local/java
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH

source /etc/profile

java -version
mvn -v

cd /usr/local/docker/myproject

# 






# docker file
FROM tomcat

WORKDIR /usr/local/tomcat/webapps/ROOT

RUN rm -fr *

ADD myshop.tar.gz /usr/local/tomcat/webapps/ROOT

RUN rm -fr myshop.tar.gz

WORDIR /usr/local/tomcat


# 保存退出
$ docker build -t 192.168.1.254:5000/myshop .
$ docker push 192.168.1.254:5000/myshop
```