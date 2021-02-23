## Nexus 依赖管理平台

### 1、什么是Nexus

Nexus是一个强大的Maven仓库管理器，它极大地简化了自己内部仓库的维护和外部仓库的访问。

利用Nexus你可以只在一个地方就能够完全控制访问 和部署在你所维护仓库中的每个Artifact。Nexus是一套“开箱即用”的系统不需要数据库，它使用文件系统加Lucene来组织数据。Nexus 使用ExtJS来开发界面，利用Restlet来提供完整的REST APIs，通过m2eclipse与Eclipse集成使用。Nexus支持WebDAV与LDAP安全身份认证。

2016年4月6日 Nexus3.0版本发布，相较于2.x版本有了很大的改变

- 对底层代码进行了大规模的重构，提升性能，增加可扩展性以及改善用户体验
- 升级界面，极大的简化了用户界面的操作和管理
- 提供新的安装包，让部署更加简单
- 增加对Docker 、NeGet、npm、Bower的支持
- 提供新的管理接口，以及增强对自动任务的管理


> 阿里云镜像
```
http://maven.aliyun.com/nexus
http://maven.aliyun.com/nexus
```

### 2、基于Docker安装Nexus

#### 2.1 使用Docker-compose安装

```bash
# 拉取docker镜像
docker pull sonatype/nexus3
docker pull sonatype/nexus3
```



```yaml
version: '3.1'
services:
  nexus:
    restart: always
    image: sonatype/nexus3
    container_name: nexus3
    ports:
      - 8081:8081
    volumes:
      - /usr/local/docker/nexus/data:/nexus-data
```

```bash
docker-compose up 
docker-compose up -d

# 如果已经初试过数据卷 需要删掉
rm -fr data/ 

# 删除docker 镜像文件
docker images
docker rmi ae52f07e5064

# 启动报错
ERROR: for nexus  Cannot create container for service nexus: invalid volume specification: '/usr/local/docker/nexus/data:nexus-data:rw': invalid mount config for type "bind": invalid mount path: 'nexus-data' mount path must be absolute
ERROR: Encountered errors while bringing up the project.
# 问题处理 
/usr/local/docker/nexus/data:nexus-data 改为
/usr/local/docker/nexus/data:/nexus-data

mkdir data
chmod 777 data

## 安装htop 查看内存使用
yum install htop
apt-get install htop
```



#### 2.2 登录控制台验证安装

```bash
http://192.168.0.108:8081
用户名 admin
密码 位于安装目录/data/admin.password 
#通过 vim admin.password  或 cat admin.password 查看
#登录后可以自行修改
admin123

#菜单
# System/API

# Security/Users
目录可以修改密码


```

### 3、Maven仓库介绍

### 4、在项目中使用Maven私服

##### 配置认证信息

在Maven Settings.xml 中添加Nexus认证信息(servers节点下)

```xml
<server>
    <id>nexus-releases</id>
    <username>admin</username>
    <password>admin123</password>
</server>
<server>
    <id>nexus-snapsots</id>
    <username>admin</username>
    <password>admin123</password>
</server>

```

##### Snapshot 与Releases的区别

- nexus-release: 用于发布Release版本
- nexus-snapshots： 用于发布Snapshot版本 快照版本

Release版本与Snapshot定义如下：

```bash
Release: 1.0.0/1.0.0-RELEASE
Snapshot: 1.0.0-SNAPSHOT
```



- 在项目pom.xml 中设置的版本号添加SNAPSHOT标识的都会发布为SNAPSHOT版本，没有SNAPSHOT标识的都会发布为RELEASE版本。
- SNAPSHOT 版本会自动增加一个世界作为标识，如 1.0.0-SNAPSHOT 发布后会变为1.0.0-SNAPSHOT-20180522.123456.1.jar

#### 配置自动化部署

在maven项目pom.xml中添加

```xml
<!--定义snapshots库和releases库的nexus地址-->  
<distributionManagement>  
    <repository>  
        <id>nexus-releases</id>  
        <url>
            http://192.168.0.108:8081/repository/maven-releases/
        </url>  
    </repository>  
    <snapshotRepository>  
        <id>nexus-snapshots</id>  
        <url>  
            http://192.168.0.108:8081/repository/maven-snapshots/  
        </url>  
    </snapshotRepository>  
</distributionManagement>  
```

```
<!-- 使用分发管理将本项目打成jar包，直接上传到指定服务器 -->
  <distributionManagement>
    <!--正式版本-->
    <repository>
        <!-- nexus服务器中用户名：在settings.xml中<server>的id-->
        <id>maven-releases</id>
        <!-- 这个名称自己定义 -->
        <name>Nexus Release Repository</name>
        <url>http://192.168.1.109:8081/repository/maven-releases/</url>
    </repository>
    <!--快照
    <snapshotRepository>
        <id>maven-snapshots</id>
        <name>Nexus Snapshot Repository</name>
        <url>http://192.168.1.109:8081/repository/maven-snapshots/</url>
    </snapshotRepository>-->
  </distributionManagement>
 
<!--仓库路径 -->
<repositories>
    <repository>
        <id>maven-public</id>
        <name>Public Repository of XXX</name>
        <url>http://192.168.1.252:8081/repository/maven-public/</url>
        <releases>
            <enabled>true</enabled>
        </releases>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
</repositories>
```

注意事项：

- ID名称必须要与settings.xml中的Services配置的ID名称保持一致
- 项目版本编号中有SNAPSHOT标识的，会发布到Nexus Snapshots Repository, 否则会发布到Nexus Release Respository , 并根据ID去匹配授权账号。

#### 部署都仓库

```bash
# 将jar包 推送到 私服仓库
mvn deploy

## 发行版 不能修改版本号
  1.0.0
  1.0.1
## 快照版本
  每次发行自动增加时间戳 和当前日期的发行次数
  
## Idea settings 配置 Maven 配置
  总是更新快照
```



#### 上传第三方JAR包

Nexus 3.23 可以使用页面上传

​		页面地址 http://192.168.1.252:8081/#browse/upload

​		页面地址 http://192.168.0.108:8081/#browse/upload

Nexus 3.0 不支持页面上传，可以使用maven命令：

```bash
mvn deploy:deploy-file 
  -DgroupId=com.vip.vfc 
  -DartifactId=common 
  -Dversion=1.0.0 
  -Dpackaging=jar 
  -Dfile=E:\common.jar 
  -Durl=http://192.168.1.252:8081/repositories/maven-releases 
  -Durl=http://192.168.1.252:8081/repositories/maven-3rd    # 第三方jar包地址 
  -DrepositoryId=nexus-releases
```

#### 配置代理仓库

从私服下载jar包

```xml
<repositories>
    <repositorie>
	    <id>nexus</id>
        <name>Nexus Repositor</name>
        <url>http://192.168.0.108:8081/repository/maven-public/</url>
        <snapshots>
        	<enabled>true</enabled>
        </snapshots>
        <releases>
        	<enabled>true</enabled>
        </releases>
    </repositorie>    
</repositories>
<pluginRepositories>
     <pluginRepository>
      <id>nexus</id>
      <name>Nexus Plugin Repository</name>
      <url>http://192.168.0.108:8081/repository/maven-public/</url>
      <releases>
        <enabled>true</enabled>
      </releases>
      <snapshots>
        <enabled>true</enabled>
      </snapshots>    
    </pluginRepository>
</pluginRepositories>
```



### 5. 部署Docker Registry WebUI



### 6 配置

#### 6.1 获取密码

nexus3之前是固定的：admin/admin123

nexus3修改了，变成启动时随机的，存放在data目录的admin.password文件

##### 6.1.1 进入容器内部 查看，密码

```bash
docker ps  

# 查看sonatype/nexus3 的容器id
docker exec -it 0266268ad5e3 /bin/bash

cd /nexus-data
ls -al
cat admin.password
```

##### 6.1.2  无需进入容器，查看密码

由于容器创建的时候已经做了数据卷映射，所以无需进入容器内部也可以查看

```
    volumes:   # 数据卷映射
      - /usr/local/docker/nexus/data:/nexus-data
```



```bash
cd /usr/local/docker/nexus/data
cat admin.password
```

#### 6.2 配置密码

获取到密码之后，使用这个密码登录 http://192.168.1.252:8081/

点击首页 右上角 Sign In 登录，输入账号admin  密码 a76384f5-a1a1-442a-a61f-51bf0c000e48 （上面查看到并复制的），登录





#### 6.3 配置从阿里云下载 jar包

配置阿里云代理

https://blog.csdn.net/ziwuzhulin/article/details/103218472

https://blog.csdn.net/ziwuzhulin/article/details/103218472