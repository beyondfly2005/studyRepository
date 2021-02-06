#### idea部署到docker的两种方式

- 使用mavne 插件插件部署

- 编写dockerfile部署



#### 使用maven插件部署

优点：不需要编写 dockerfile



#### 两款maven-docker插件



```xml
<groupId>io.fabric8</groupId>
<artifactId>docker-maven-plugin</artifactId>
```

```xml
<groupId>com.spotify</groupId>
<artifactId>docker-maven-plugin</artifactId>
```

#### 两款插件的配置

```xml
<plugin>
    <groupId>io.fabric8</groupId>
    <artifactId>docker-maven-plugin</artifactId>
    <version>0.26.0</version>

    <!--全局配置-->
    <configuration>
        <!--这一部分是为了实现对远程docker容器的控制-->
        <!--docker主机地址,用于完成docker各项功能,注意是tcp不是http!-->
        <dockerHost>tcp://192.168.01.252:2376</dockerHost>
        <!--docker远程访问所需证书地址,如果docker远程主机没有启用TLS验证则不需要配证书-->
        <certPath>${project.basedir}/docker/ssh</certPath>

        <!--这一部分是为了实现docker镜像的构建和推送-->
        <!--registry地址,用于推送,拉取镜像,我这里用的是阿里的registry-->
        <registry>registry.cn-shenzhen.aliyuncs.com</registry>
        <!--认证配置,用于私有registry认证,如果忘记了可以去阿里的registry查看-->
        <authConfig>
            <push>
                <username>这里填registry的用户名</username>
                <password>这里填registry的密码</password>
            </push>
        </authConfig>

        <!--镜像相关配置,支持多镜像-->
        <images>
            <!-- 单个镜像配置 -->
            <image>
                <!--镜像名(含版本号)-->
                <name>命名空间/仓库名称:镜像版本号</name>
                <!--别名:用于容器命名和在docker-compose.yml文件只能找到对应名字的配置-->
                <alias>${project.name}</alias>
                <!--镜像build相关配置-->
                <build>
                    <!--使用dockerFile文件-->
                    <dockerFile>${project.basedir}/docker/${project.name}</dockerFile>
                </build>
                <!--配置docker-compose文件-->
                <external>
                    <type>compose</type>
                    <basedir>${project.basedir}/docker</basedir>
                    <composeFile>docker-compose.yml</composeFile>
                </external>
                <!--容器run相关配置-->
                <run>
                    <!--配置运行时容器命名策略为:别名,如果不指定则默认为none,即使用随机分配名称-->
                    <namingStrategy>alias</namingStrategy>
                </run>
            </image>
        </images>
    </configuration>
    <dependencies>
        <!--该插件需要这个依赖-->
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>1.11</version>
        </dependency>
    </dependencies>
</plugin>
```



#### 使用 com.spotify.docker-maven-plugin 打包发布

```
使用idea右侧 maven package 打包 自动进行docker部署
```

##### 错误1：mavne打包失败

```
repackage failed: Unable to find a single main class from the following candidates [com.ac.intellsecurity.uaa.UaaServerApplication, com.intellsecurity.uaa.uaa.UaaServerApplication]
```

##### 解决方案

```
方法1:

在util工具类中 有其他的main 函数，将其注释 或者删除即可

方法2：

在idea右侧 maven中 clean 项目
```

##### 错误2：发布到远程docker失败

```
com.spotify.docker.client.shaded.org.apache.http.impl.execchain.RetryExec execute
信息: I/O exception (java.net.SocketException) caught when processing request to {}->http://192.168.1.252:2375: Connection reset by peer: socket write error
```

##### 解决方案

把要构建的镜像名称全部改为小写，这里配置为<imageName>${docker.image.prefix}/${project.artifactId}</imageName>,所以要保证项目的artifactId为小写。

##### 参考文档 ：https://blog.csdn.net/sinat_36553913/article/details/89003580





#### 参考文档

https://www.cnblogs.com/gaomanito/p/12027778.html

https://blog.csdn.net/qq_40298902/article/details/106543208

