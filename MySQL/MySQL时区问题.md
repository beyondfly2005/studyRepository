

##### 查看mysql当前时间，当前时区

```sql
select curtime();  
#或
select now()


show variables like "%time_zone%";

```



##### 修改时区

```mysql
##修改mysql全局时区为北京时间，即我们所在的东8区
set global time_zone = '+8:00'; 
##修改当前会话时区
set time_zone = '+8:00'; 
#立即生效
flush privileges;
```



##### 通过修改my.cnf配置文件来修改时区

```bash
# vim /etc/my.cnf ##在[mysqld]区域中加上
default-time_zone = '+8:00'
# /etc/init.d/mysqld restart ##重启mysql使新时区生效
```



##### 如果不方便重启mysql，又想临时解决时区问题，可以通过php或其他语言在初始化mysql时初始化mysql时区

以php为例，在mysql_connect()下使用:

```
mysql_query("SET time_zone = '+8:00'")
```



##### mysql时区转换函数

```sql
convert_tz(dt,from_tz,to_tz)
select convert_tz('2008-08-08 12:00:00', '+08:00', '+00:00'); -- 2008-08-08 04:00:00
-- 时区转换也可以通过 date_add, date_sub, timestampadd 来实现。
select date_add('2008-08-08 12:00:00', interval -8 hour); -- 2008-08-08 04:00:00
select date_sub('2008-08-08 12:00:00', interval 8 hour); -- 2008-08-08 04:00:00
select timestampadd(hour, -8, '2008-08-08 12:00:00'); -- 2008-08-08 04:00:00
```



##### 时间格式

GMT（Greenwich Mean Time）：格林威治标准时间
UTC：世界标准时间
CST（China Standard Time）：中国标准时间

GMT + 8 = UTC + 8 = CST



##### Dokcer容器内的mysql

```bash
#进入容器
docker exec -it mysql bash

#
cd /etc/mysql/mysql.conf.d
vim mysqld.cnf

#重启容器内的mysql
service mysql restart
#重启docker
docker restart mysql
```

