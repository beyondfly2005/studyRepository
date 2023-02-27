# CentOS防火墙配置

centos6 使用的是iptables防火墙
centos7使用的是用firewalld代替了原来的iptablese


## centOS6 Iptables防火墙

--停止防火墙 停止iptables服务
```bash
service iptables stop
```


--查看iptables状态
```bash
/etc/init.d/iptables status
```

--配置iptables策略
```bash
vi /etc/sysconfig/iptables
```

添加：
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8090 -j ACCEPT
:wq

重启iptables服务即可

--iptables重启
service iptables restart

开机自启动
chkconfig iptables on

开机不启动
chkconfig iptables off


## centos7 firewalld防火墙

1、查看firewall服务状态
```bash
systemctl status firewalld
```

2、查看firewall的状态
```bash
firewall-cmd --state
```

3、开启、重启、关闭、firewalld.service服务
```bash
# 开启
service firewalld start
# 重启
service firewalld restart
# 关闭
service firewalld stop
```

4、查看防火墙规则
```
firewall-cmd --list-all
``` 

5、查询、开放、关闭端口
```
# 查询端口是否开放
firewall-cmd --query-port=8080/tcp
# 开放80端口
firewall-cmd --permanent --add-port=80/tcp
# 移除端口
firewall-cmd --permanent --remove-port=8080/tcp
```

#重启防火墙(修改配置后要重启防火墙)
```
firewall-cmd --reload

# 参数解释
1、firwall-cmd：是Linux提供的操作firewall的一个工具；
2、--permanent：表示设置为持久；
3、--add-port：标识添加的端口；
```

6、开放oracle1521端口
firewall-cmd --zone=public --add-port=1521/tcp --permanent
--重新加载
firewall-cmd --reload








安装Firewall命令：
yum install firewalld firewalld-config

Firewall开启常见端口命令：

--开放80端口
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --add-port=8080/tcp --permanent

firewall-cmd --zone=public --add-port=443/tcp --permanent

firewall-cmd --zone=public --add-port=22/tcp--permanent

firewall-cmd --zone=public--add-port=21/tcp --permanent

firewall-cmd --zone=public--add-port=53/udp --permanent

Firewall关闭常见端口命令：

firewall-cmd --zone=public--remove-port=80/tcp --permanent

firewall-cmd --zone=public--remove-port=443/tcp --permanent

firewall-cmd --zone=public--remove-port=22/tcp --permanent

firewall-cmd --zone=public--remove-port=21/tcp --permanent

firewall-cmd --zone=public--remove-port=53/udp --permanent

批量添加区间端口

firewall-cmd --zone=public--add-port=4400-4600/udp --permanent

firewall-cmd --zone=public--add-port=4400-4600/tcp --permanent

开启防火墙命令：

systemctl start firewalld.service

重启防火墙命令：

firewall-cmd --reload  或者   service firewalld restart

查看端口列表：

firewall-cmd --permanent --list-port

禁用防火墙

systemctl stop firewalld

设置开机启动

systemctl enable firewalld

停止并禁用开机启动

sytemctl disable firewalld

查看状态

systemctl status firewalld或者firewall-cmd --state



参考文档：
https://www.cnblogs.com/xxoome/p/7115614.html
https://blog.csdn.net/u013514928/article/details/80411110
