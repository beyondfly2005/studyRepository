# CentOS7修改IP地址

#### 1、进入网络配置文件目录

```bash
 cd /etc/sysconfig/network-scripts/
```

#### 2、找到我们需要修改的配置文件

```bash
## 列出目录下的文件及目录
ll
## 网络配置文件通常以ifcfg-ens开头  例如：ifcfg-ens33

```

#### 3、修改配置文件

```bash
vim ifcfg-ens33
```

```properties
# 将dhcp改为static
BOOTPROTO=static

# 将网卡设置 为开机启用
ONBOOT=yes			

# 同时在文字下方添加
#静态IP 
IPADDR=192.168.0.108
#默认网关 
GATEWAY=192.168.0.1
#子网掩码 
NETMASK=255.255.255.0
#DNS 配置 223.5.5.5/223.6.6.6为阿里云DNS
DNS1=223.5.5.5
DNS2=223.6.6.6
```

#### 4、重启网络服务

```bash
# 重启网络服务
service network restart
```

#### 5、查看修改是否生效

```bash
ifconfig
# 或
ip addr
```

