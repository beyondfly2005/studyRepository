https://www.cnblogs.com/lkj371/p/12809195.html





# [toad安装oracle客户端过程](https://www.cnblogs.com/lkj371/p/12809195.html)

## 一、描述

使用toad连接oracle时提示找不到客户端，下面分享一下处理过程

## 二、过程

1、到ORACLE 网站下载instantclient客户端

https://www.oracle.com/database/technologies/instant-client/winx64-64-downloads.html

2、Oracle Instatnt Client解压至指定目录（自定义）如本例中放到如下目录 

D:\program\instantclient_18_5

3、配置系统变量

```
NLS_LANG = AMERICAN_AMERICA.ZHS16GBK
TNS_ADMIN = D:\program\instantclient_18_5\
LD_LIBRARY_PATH = D:\program\instantclient_18_5
SQLPATH = D:\program\instantclient_18_5ORACLE_HOME = D:\program\instantclient_18_5Path（在变量中加入） D:\program\instantclient_18_5
```

5、添加配置文件sqlnet.ora和tnsnames.ora

sqlnet.ora代码如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
# sqlnet.ora Network Configuration File: /u01/app/oracle/product/11.2.0/db_1/network/admin/sqlnet.ora
# Generated by Oracle configuration tools.

NAMES.DIRECTORY_PATH= (TNSNAMES, EZCONNECT)

ADR_BASE = D:\program\instantclient_18_5
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

tnsnames.ora代码如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
ORCL =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.233)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = orcl)
    )
  )
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

6、通过toad连接OK

##  三、其他

### 3.1 toad的下载地址