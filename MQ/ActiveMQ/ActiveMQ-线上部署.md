

### ActiveMQ-线上部署

#### 1、上传文件

上传apache-activemq-5.16.0-bin.tar.gz文件到 /usr/local/

```bash
cd /usr/local/
```

#### 2、解压tar包

```bash
## 解压tar包
tar -zxvf apache-activemq-5.16.0-bin.tar.gz

## 将解压后的文件夹重命名为activemq
mv apache-activemq-5.16.0 activemq

## 删除压缩包 以释放空间
rm -rf apache-activemq-5.16.0-bin.tar.gz
```

#### 3、配置Web控制台账号

```bash
cd conf
vim jetty-realm.properties
```

**注释掉前两行，添加最后一行**

```

#admin: admin, admin
#user: user, user
admin: Hbac*#2020!, admin
```

```bash
:wq  #保存退出
```

#### 4、配置安全机制

##### 4.1 修改 配置文件activemq.xml文件

```
cd conf
vim vim activemq.xml
```

在<broker></broker> 节点 添加以下信息

```xml
	<plugins>
	    <simpleAuthenticationPlugin>
	        <users>
	            <authenticationUser username="${activemq.username}" password="${activemq.password}" groups="users,admins"/>
	        </users>
	    </simpleAuthenticationPlugin>
	</plugins>

```

```
!wq 保存退出
```

##### 4.2 修改配置文件credentials.properties

```bash
vim credentials.properties
```



```
activemq.username=system
activemq.password=manager

##修改密码
activemq.password=Hbac2020!
```
```
!wq 保存退出
```



#### 5、启动/停止/重启服务

##### 5.1 启动服务

```bash
cd /usr/local/activemq/bin
./activemq start


# 如果是第一次启动 需要执行两次这个命令  第一次生成配置信息，第二次启动服务
./activemq start
./activemq start
```



##### 5.2 重启服务

```bash
cd /usr/local/activemq/bin
./activemq restart
```



##### 5.3 停止服务

```bash
cd /usr/local/activemq/bin
./activemq stop
```

