### 上传安装包到服务器


### 解压tar包 解压到指定路径usr/local/openoffice
```bash
tar xf Apache_OpenOffice_4.1.7_Linux_x86-64_install-rpm_zh-CN.tar.gz -C /usr/local/openoffice
cd /usr/local/openoffice/zh-CN/RPMS
```



### 安装相关的rpm包
```bash
rpm -ivh openoffice-*
```



### 默认安装目录 /opt/openoffice4/program 中
```bash
#执行 
/opt/openoffice4/program/soffice -accept="socket,host=127.0.0.1,port=8100;urp;" -headless -nofirststartwizard &
/opt/openoffice4/program/soffice -accept="socket,host=127.0.0.1,port=8100:urp;" -headless 
#或者
cd /opt/openoffice4/program/
./soffice -headless -accept="socket,host=127.0.0.1,port=8100:urp;" -nofirststartwizard & 
#在命令的最后输入 & 可确保服务在后端运行
```

### 查看openoffice进程
```bahs
ps -ef|grep soffice  或
ps -ef|grep openoffice
```



### 启动之后 在program目录输入
```bash
netstat –tln查看是否启动成功！如上图所示有8100这个端口就可以使用了
netstat -lnp |grep 端口号
```

