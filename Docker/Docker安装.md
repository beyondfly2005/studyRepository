> 参考文档
> https://blog.csdn.net/AkiraNicky/article/details/85775076

### 1、Docker安装
##### 查看是否已经安装docker

```bash
$ docker version
```

##### Docker在 CentOS7上的安装


###### ①  检查内核版本

Docker 要求 CentOS 系统的内核版本在 3.10以上，所有需要检查内核版本 通过uname -r 命令查看你当前的内核版本
```bash
$ uname -r
```
###### ② 更新yum
 使用 root 权限登录 Centos。确保 yum 包更新到最新
```bash
$ yum -y update
```

###### ③ 卸载旧版本

(如果安装过旧版本的话，需要卸载；如果没安装过，跳过这一步)
```bash
$ yum remove docker docker-common docker-selinux docker-engine
```

###### ④ 安装需要的软件包

 yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的

```bash
$ yum install -y yum-utils device-mapper-persistent-data lvm2
```

###### ⑤ 设置yum源

```bash
$ yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

###### ⑥ 选择版本

可以查看所有仓库中所有docker版本，并选择特定版本安装

```bash
$ yum list docker-ce --showduplicates | sort -r
```
###### ⑦ 安装docker

```bash
sudo yum install -y docker-ce     
#由于repo中默认只开启stable仓库，故这里安装的是最新稳定版18.03.1
sudo yum install -y docker-ce docker-ce-cli containerd.io  
#安装最新版本的 Docker Engine-Community 和 containerd
sudo yum install docker-ce docker-ce-cli containerd.io
```

###### ⑧ 启动 Docker

```bash
systemctl start docker
```

###### ⑨加入开机启动

```bash
systemctl enable docker
```

###### ⑩ 验证安装是否成功

(有client和server两部分表示docker安装启动都成功了)

```bash
docker version
```

### 2、Docker配置镜像加速

> 加速配置参考
> ​https://www.jianshu.com/p/a024dc5ade92



```bash
# 国内常用的镜像加速服务器
# Docker官方提供的中国镜像库：https://registry.docker-cn.com
# 七牛云加速器：https://reg-mirror.qiniu.com
# 在centos7中配置镜像加速(Ubuntu16.04+、Debian8+、CentOS7 与此相同，同为使用 systemd 的系统)
$ vim /etc/docker/daemon.json 
# 增加以下内容
{
  "registry-mirrors":["https://registry.docker-cn.com"]
}
# 或者
{
  "registry-mirrors": ["http://hub.mirror.c.163.com"]
}

# 之后重新启动服务：
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

### 3、Docker下载镜像文件

###### 	① 搜索镜像 

​	在Docker Hub中搜索含有 java 关键词的镜像仓库
```bash
docker search java
```
​	在Docker Hub中搜索含有 jhello-world 关键词的镜像仓库
```bash
docker search hello-world
```

###### ②下载镜像

这里以下载java8为例

```
docker pull java:8
```
下载hello-world仓库
```
docker pull hello-world
```

###### ③ 列出本地镜像

已下载的镜像

```
docker images
```

	11 运行 hello-world 映像来验证是否正确安装了 Docker Engine-Community 
	sudo docker run hello-world

### 4、Docker常用命令

```
docker images  查看本地镜像文件
	
docker ps  查看正在运行的容器
	
docker ps –a  查看所有的容器
	
docker container exec -it container_id /bin/bash  进入到容器
	
exit  退出（docker容器中的命令，键入此命令，退回宿主机命令行）
	
docker version  查看版本
	
docker run -d -p 80:80 nginx  启动nginx容器
	
docker rmi imgage_id  删除镜像（docker imges查出来的结果）
	
docker rm 容器id  删除容器
	
docker stop 容器name  停止容器

```

### 5、常用的服务安装

```bash
Docker安装Tomcat

## 方法一、docker pull tomcat
docker pull tomcat	#安装最新版本的tomcat
docker search tomcat	#在镜像库中搜索tomcat可用版本

## 本地镜像列表里查找tomcat
docker images|grep tomcat

## 方法二、通过 Dockerfile 构建
mkdir -p ~/tomcat/webapps ~/tomcat/logs ~/tomcat/conf
webapps 目录将映射为 tomcat 容器配置的应用程序目录。
logs 目录将映射为 tomcat 容器的日志目录。
conf 目录里的配置文件将映射为 tomcat 容器的配置文件。

##进入创建的 tomcat 目录，创建 Dockerfile。
cd tomcat
touch Dockerfile
vim Dockerfile
```
```dockerfile	
FROM openjdk:8-jre
​```bash
ENV CATALINA_HOME /usr/local/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH
RUN mkdir -p "$CATALINA_HOME"
WORKDIR $CATALINA_HOME

# let "Tomcat Native" live somewhere isolated
ENV TOMCAT_NATIVE_LIBDIR $CATALINA_HOME/native-jni-lib
ENV LD_LIBRARY_PATH ${LD_LIBRARY_PATH:+$LD_LIBRARY_PATH:}$TOMCAT_NATIVE_LIBDIR

# runtime dependencies for Tomcat Native Libraries
# Tomcat Native 1.2+ requires a newer version of OpenSSL than debian:jessie has available
# > checking OpenSSL library version >= 1.0.2...
# > configure: error: Your version of OpenSSL is not compatible with this version of tcnative
# see http://tomcat.10.x6.nabble.com/VOTE-Release-Apache-Tomcat-8-0-32-tp5046007p5046024.html (and following discussion)
# and https://github.com/docker-library/tomcat/pull/31
ENV OPENSSL_VERSION 1.1.0f-3+deb9u2
RUN set -ex; \
    currentVersion="$(dpkg-query --show --showformat '${Version}\n' openssl)"; \
    if dpkg --compare-versions "$currentVersion" '<<' "$OPENSSL_VERSION"; then \
        if ! grep -q stretch /etc/apt/sources.list; then \
# only add stretch if we're not already building from within stretch
            { \
                echo 'deb http://deb.debian.org/debian stretch main'; \
                echo 'deb http://security.debian.org stretch/updates main'; \
                echo 'deb http://deb.debian.org/debian stretch-updates main'; \
            } > /etc/apt/sources.list.d/stretch.list; \
            { \
# add a negative "Pin-Priority" so that we never ever get packages from stretch unless we explicitly request them
                echo 'Package: *'; \
                echo 'Pin: release n=stretch*'; \
                echo 'Pin-Priority: -10'; \
                echo; \
# ... except OpenSSL, which is the reason we're here
                echo 'Package: openssl libssl*'; \
                echo "Pin: version $OPENSSL_VERSION"; \
                echo 'Pin-Priority: 990'; \
            } > /etc/apt/preferences.d/stretch-openssl; \
        fi; \
        apt-get update; \
        apt-get install -y --no-install-recommends openssl="$OPENSSL_VERSION"; \
        rm -rf /var/lib/apt/lists/*; \
    fi

RUN apt-get update && apt-get install -y --no-install-recommends \
        libapr1 \
    && rm -rf /var/lib/apt/lists/*

# see https://www.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/KEYS
# see also "update.sh" (https://github.com/docker-library/tomcat/blob/master/update.sh)
ENV GPG_KEYS 05AB33110949707C93A279E3D3EFE6B686867BA6 07E48665A34DCAFAE522E5E6266191C37C037D42 47309207D818FFD8DCD3F83F1931D684307A10A5 541FBE7D8F78B25E055DDEE13C370389288584E7 61B832AC2F1C5A90F0F9B00A1C506407564C17A3 713DA88BE50911535FE716F5208B0AB1D63011C7 79F7026C690BAA50B92CD8B66A3AD3F4F22C4FED 9BA44C2621385CB966EBA586F72C284D731FABEE A27677289986DB50844682F8ACB77FC2E86E29AC A9C5DF4D22E99998D9875A5110C01C5A2F6059E7 DCFD35E0BF8CA7344752DE8B6FB21E8933C60243 F3A04C595DB5B6A5F1ECA43E3B7BBB100D811BBE F7DA48BB64BCB84ECBA7EE6935CD23C10D498E23

ENV TOMCAT_MAJOR 8
ENV TOMCAT_VERSION 8.5.32
ENV TOMCAT_SHA512 fc010f4643cb9996cad3812594190564d0a30be717f659110211414faf8063c61fad1f18134154084ad3ddfbbbdb352fa6686a28fbb6402d3207d4e0a88fa9ce

ENV TOMCAT_TGZ_URLS \
# https://issues.apache.org/jira/browse/INFRA-8753?focusedCommentId=14735394#comment-14735394
    https://www.apache.org/dyn/closer.cgi?action=download&filename=tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz \
# if the version is outdated, we might have to pull from the dist/archive :/
    https://www-us.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz \
    https://www.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz \
    https://archive.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz

ENV TOMCAT_ASC_URLS \
    https://www.apache.org/dyn/closer.cgi?action=download&filename=tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc \
# not all the mirrors actually carry the .asc files :'(
    https://www-us.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc \
    https://www.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc \
    https://archive.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz.asc

RUN set -eux; \
    \
    savedAptMark="$(apt-mark showmanual)"; \
    apt-get update; \
    \
    apt-get install -y --no-install-recommends gnupg dirmngr; \
    \
    export GNUPGHOME="$(mktemp -d)"; \
    for key in $GPG_KEYS; do \
        gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
    done; \
    \
    apt-get install -y --no-install-recommends wget ca-certificates; \
    \
    success=; \
    for url in $TOMCAT_TGZ_URLS; do \
        if wget -O tomcat.tar.gz "$url"; then \
            success=1; \
            break; \
        fi; \
    done; \
    [ -n "$success" ]; \
    \
    echo "$TOMCAT_SHA512 *tomcat.tar.gz" | sha512sum -c -; \
    \
    success=; \
    for url in $TOMCAT_ASC_URLS; do \
        if wget -O tomcat.tar.gz.asc "$url"; then \
            success=1; \
            break; \
        fi; \
    done; \
    [ -n "$success" ]; \
    \
    gpg --batch --verify tomcat.tar.gz.asc tomcat.tar.gz; \
    tar -xvf tomcat.tar.gz --strip-components=1; \
    rm bin/*.bat; \
    rm tomcat.tar.gz*; \
    rm -rf "$GNUPGHOME"; \
    \
    nativeBuildDir="$(mktemp -d)"; \
    tar -xvf bin/tomcat-native.tar.gz -C "$nativeBuildDir" --strip-components=1; \
    apt-get install -y --no-install-recommends \
        dpkg-dev \
        gcc \
        libapr1-dev \
        libssl-dev \
        make \
        "openjdk-${JAVA_VERSION%%[.~bu-]*}-jdk=$JAVA_DEBIAN_VERSION" \
    ; \
    ( \
        export CATALINA_HOME="$PWD"; \
        cd "$nativeBuildDir/native"; \
        gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"; \
        ./configure \
            --build="$gnuArch" \
            --libdir="$TOMCAT_NATIVE_LIBDIR" \
            --prefix="$CATALINA_HOME" \
            --with-apr="$(which apr-1-config)" \
            --with-java-home="$(docker-java-home)" \
            --with-ssl=yes; \
        make -j "$(nproc)"; \
        make install; \
    ); \
    rm -rf "$nativeBuildDir"; \
    rm bin/tomcat-native.tar.gz; \
    \
# reset apt-mark's "manual" list so that "purge --auto-remove" will remove all build dependencies
    apt-mark auto '.*' > /dev/null; \
    [ -z "$savedAptMark" ] || apt-mark manual $savedAptMark; \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
    rm -rf /var/lib/apt/lists/*; \
    \
# sh removes env vars it doesn't support (ones with periods)
# https://github.com/docker-library/tomcat/issues/77
    find ./bin/ -name '*.sh' -exec sed -ri 's|^#!/bin/sh$|#!/usr/bin/env bash|' '{}' +

# verify Tomcat Native is working properly
RUN set -e \
    && nativeLines="$(catalina.sh configtest 2>&1)" \
    && nativeLines="$(echo "$nativeLines" | grep 'Apache Tomcat Native')" \
    && nativeLines="$(echo "$nativeLines" | sort -u)" \
    && if ! echo "$nativeLines" | grep 'INFO: Loaded APR based Apache Tomcat Native library' >&2; then \
        echo >&2 "$nativeLines"; \
        exit 1; \
    fi

EXPOSE 8080
CMD ["catalina.sh", "run"]

```

```bash
# 通过 Dockerfile 创建一个镜像，替换成你自己的名字
~/tomcat$ docker build -t tomcat .
	
# 创建完成后，我们可以在本地的镜像列表里查找到刚刚创建的镜像：
runoob@runoob:~/tomcat$ docker images|grep tomcat
	
# 使用 tomcat 镜像
# 运行容器
~/tomcat$ docker run --name tomcat -p 8080:8080 -v $PWD/test:/usr/local/tomcat/webapps/test -d tomcat  
	
# 查看容器启动情况
docker ps 

```