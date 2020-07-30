

### 多模块项目发布

#### 编写SSH脚本

```bash
cd /root/.jenkins/workspace/IntellSecurity-truk
cp IntellSecurity-Web/target/IntellSecurity-web.war /home/sysac/apache-tomcat-7.0.99/webapps
cp IntellSecurity-App/target/IntellSecurity-App.war /home/sysac/apache-tomcat-7.0.99/webapps
cp IntellSecurity-Monitor/target/IntellSecurity-Monitor.war  /home/sysac/apache-tomcat-7.0.99/webapps
cp IntellSecurity-Monitor/target/IntellSecurity-Restfull.war  /home/sysac/apache-tomcat-7.0.99/webapps
cp IntellSecurity-Monitor/target/IntellSecurity-CronJob.war  /home/sysac/apache-tomcat-7.0.99/webapps
#或者
cp IntellSecurity-*/target/IntellSecurity-*.war /home/sysac/apache-tomcat-7.0.99/webapps/

cd /home/sysac/apache-tomcat-7.0.99/bin
sh ./shutdown.sh
sh startup.sh

# 如果进程卡住了怎么办 kill -9 pid
# ps -ef|grep $tomcat_home|grep -v grep|awk
# ps -ef|grep  /home/sysac/apache-tomcat-7.0.99|grep -v grep|awk
# ps -ef|grep  /home/sysac/apache-tomcat-7.0.99
tomcat_home=/home/sysac/apache-tomcat-7.0.99
pid=$(ps -ef | grep $tomcat_home | grep -v grep | awk '{print $2}')
if [ ${pid} ]
then 
	kill -9 ${pid}
	echo "Kill ${pid} Success!"
	echo  "重启中。。。。。。。"
    sh startup.sh
else     
    echo  "重启中... ... ..."
    sh startup.sh
fi

```

> 以下为测试发布的最终版 已经通过测试

```bas
cd /root/.jenkins/workspace/IntellSecurity-truk
cp IntellSecurity-Web/target/IntellSecurity-web.war /home/sysac/apache-tomcat-7.0.99/webapps
cp IntellSecurity-App/target/IntellSecurity-App.war /home/sysac/apache-tomcat-7.0.99/webapps

#或者
#cp IntellSecurity-*/target/IntellSecurity-*.war /home/sysac/apache-tomcat-7.0.99/webapps/

cd /home/sysac/apache-tomcat-7.0.99/bin
sh ./shutdown.sh
#sh ./startup.sh

# 如果进程卡住了怎么办 kill -9 pid
tomcat_home=/home/sysac/apache-tomcat-7.0.99
pid=$(ps -ef | grep $tomcat_home | grep -v grep | awk '{print $2}')
if [ ${pid} ]
then 
	kill -9 ${pid}
	echo "Kill ${pid}  Success!"
	echo  "重启中。。。。。。。"
    sh startup.sh
else 
    echo  "重启中... ... ..."
    sh startup.sh
fi
```



#### SSH远程发布

> 首先要实现ssh免密登录

```bash
# 在客户端执行
ssh-keygen -t rsa 
#/home/userXX/.ssh目录下生成密钥对
ls -la .ssh
ssh-copy-id root@192.168.1.252
#确认yes后 输入密码lam123
#提示成功使用 ssh 'root@192.168.1.252' 远程连接
```

> 远程发布ssh脚本

```bash
cd /root/.jenkins/workspace/IntellSecurity-truk
scp IntellSecurity-Web/target/IntellSecurity-web.war root@192.168.1.252:/usr/local/apache-tomcat-7.0.99/webapps
scp IntellSecurity-App/target/IntellSecurity-App.war root@192.168.1.252:/usr/local/apache-tomcat-7.0.99/webapps
scp IntellSecurity-Monitor/target/IntellSecurity-Monitor.war root@192.168.1.252:/usr/local/apache-tomcat-7.0.99/webapps
scp IntellSecurity-Monitor/target/IntellSecurity-Restfull.war root@192.168.1.252:/usr/local/apache-tomcat-7.0.99/webapps
#scp IntellSecurity-Monitor/target/IntellSecurity-CronJob.war root@192.168.1.252:/usr/local/apache-tomcat-7.0.99/webapps
ssh -t -t root@192.168.1.252
cd /usr/local/apache-tomcat-7.0.99/bin
sh ./shutdown.sh
tomcat_home=/usr/local/apache-tomcat-7.0.99
pid=$(ps -ef | grep $tomcat_home | grep -v grep | awk '{print $2}')
if [ ${pid} ]
then 
	kill -9 ${pid}
	echo "Kill ${pid}  Success!"
	echo  "重启中。。。。。。。"
    sh ./startup.sh
else     
    echo  "重启中... ... ..."
    sh ./startup.sh
fi
exit

```

> ssh 远程登录 会报错：
>
> ```
> Pseudo-terminal will not be allocated because stdin is not a terminal
> ```



> 解决方法  加入 -t -t 参数 或者 -tt参数 如下
>
> ssh -t -t user@10.242.1.1
>
> 或
>
> ssh user@10.242.1.1 -tt



> 安装使用openOffice实现doc转pdf后，tomcat启动时 会同时启动多个进程 来进行文档转换
>
> 此时 ps -ef | grep tomcat 会有多个端口多个进程，应该将这些端口进程占用 全部kill掉 

```bash
# 这段脚本 将返回多个
if [ ${pid} ]
then 
	kill -9 ${pid}
else
fi
```

改为

```bash
for pid in ${pids}
do
    pid_num=${pid}
    echo "pid_num:${pid_num}"
    #count=`expr ${count}+1`
    count=$(($count+1))
    echo  "count: ${count}"
done
```

```bash

cd /root/.jenkins/workspace/IntellSecurity-truk
scp IntellSecurity-Web/target/IntellSecurity-web.war root@192.168.1.252:/usr/local/apache-tomcat-7.0.99/webapps
scp IntellSecurity-App/target/IntellSecurity-App.war root@192.168.1.252:/usr/local/apache-tomcat-7.0.99/webapps
scp IntellSecurity-Monitor/target/IntellSecurity-Monitor.war root@192.168.1.252:/usr/local/apache-tomcat-7.0.99/webapps
scp IntellSecurity-Restfull/target/IntellSecurity-Restfull.war root@192.168.1.252:/usr/local/apache-tomcat-7.0.99/webapps
#scp IntellSecurity-CronJob/target/IntellSecurity-CronJob.war root@192.168.1.252:/usr/local/apache-tomcat-7.0.99/webapps
ssh -t -t root@192.168.1.252
cd /usr/local/apache-tomcat-7.0.99/bin
sh ./shutdown.sh
tomcat_home=/usr/local/apache-tomcat-7.0.99
pids=$(ps -ef | grep $tomcat_home | grep -v grep | awk '{print $2}')
for pid in [ ${pids} ]
do
	kill -9 ${pid}
	sleep 3
	echo "Kill ${pid}  Success!"
done
echo  "重启中... ... ..."
sh ./startup.sh

echo  "重启中。。。。。。。"
sh ./startup.sh
exit
```



#### 安装SSH 插件



将以上代码中的 重启tomcat部分 移动到远程计算机执行 

分两步执行 客户机 复制war包到远程机，远程机 重启tomcat

在远程目标机执行以下

```bash
cd /usr/local//apache-tomcat-7.0.99/bin
vim restart_tomcat.sh

# 添加以下内容

ssh -t -t root@192.168.1.252
cd /usr/local/apache-tomcat-7.0.99/bin
sh ./shutdown.sh
tomcat_home=/usr/local/apache-tomcat-7.0.99
pids=$(ps -ef | grep $tomcat_home | grep -v grep | awk '{print $2}')
for pid in [ ${pids} ]
do
	kill -9 ${pid}
	sleep 3
	echo "Kill ${pid}  Success!"
done
echo  "重启中... ... ..."
sh ./startup.sh

## wq 保存退出
# 加入执行权限
chmod +x restart_tomcat.sh

```







