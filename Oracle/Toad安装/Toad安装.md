> https://blog.csdn.net/qq_29695701/article/details/80169924



### 安装 toad

1. 下载相关软件
   下载 **toad**，里面包含了永久性激活码:
   链接:https://pan.baidu.com/s/1lwD4-Z_KPaRG7wwVX06b1w 密码:jvsa
   下载 **instantclient**:
   链接:https://pan.baidu.com/s/1keOmriXZ4CMzvVYqy3aIog 密码:uvzp
   请自行搜索下载并安装 **vcredist_x64.exe**。

2. 安装 instantclient:
   解压缩instantclient，将解压缩得到的instantclient_12_2下的所有文件放在某个路径下，比如: `c:\Program Files\Oracle\instantclient_12_2`

   > **配置环境变量**
   > win7即以上操作系统为:计算机(右键)——属性——高级系统设置——环境变量，新建三个 系统变量:
   >
   > 1. 变量名: `ORACLE_HOME`
   >    变量值: `c:\Program Files\Oracle\instantclient_12_2`
   > 2. 变量名: `TNS_ADMIN`
   >    变量值: `%ORACLE_HOME%\`
   >    *说明: 不要略去该值最后的 `\` 符号，不然可能会影响到 PLSQL*
   > 3. 变量名: `NLS_LANG`
   >    变量值: `SIMPLIFIED CHINESE_CHINA.ZHS16GBK`
   > 4. 修改Path变量，在后面添加 `%ORACLE_HOME%`

3. 安装 toad:下载后的 文件中有具体的安装说明，不再赘述。