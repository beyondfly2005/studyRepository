#### 1、下载JDK8

官网手动下载Linux环境下的jdk1.8

http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

#### 2、上传到Linux服务器

使用xftp将jdk源码包，上传到/usr/local（软件一般安装到这个目录）

#### 3、解压源码包

```bash
tar -zxvf jdk-8u221-linux-x64.tar.gz
```

解压完成删掉jdk源码包

```bash
rm -f jdk-8u221-linux-x64.tar.gz
```

#### 4、配置环境变量

```bash
vim /etc/profile
```

在profile文件尾部添加如下内容

```properties
export JAVA_HOME=/usr/local/jdk1.8.0_221  #jdk安装目录
export JRE_HOME=${JAVA_HOME}/jre 
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH
export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin 
export PATH=$PATH:${JAVA_PATH}
```

 :wq 保存并退出编辑

生效配置文件

```bash
source /etc/profile
# 让 profile文件生效
```

#### 5、测试安装是否成功

```bash
java -version
```

