Windows系统 同时安装客户端和服务器端

很容易造成各种问题

### windows下 sqlplus / as sysdba 报ora-12560的终极解决方法

https://blog.csdn.net/msdnchina/article/details/38169095





### 如何创建用户



```sql
grant sysdba to rootep;
grant resource to rootep;
grant sysdba to user;
grant imp_full_database to user;
```

### 修改密码

```
alter user scott identified by tiger;
 conn scott/tiger
```



### Oracle 11g中修改被锁定的用户

```
sqlplus / as sysdba;
alter user scott account unlock;
alter user scott identified by grace;
```



### Oracle修改服务器字符集

```sql
conn /as sysdba ;
shutdown immediate; 
startup mount 
ALTER SYSTEM ENABLE RESTRICTED SESSION; 
 ALTER SYSTEM SET JOB_QUEUE_PROCESSES=0; 
 ALTER SYSTEM SET AQ_TM_PROCESSES=0; 
 ALTER DATABASE CHARACTER SET ZHS16GBK; 
 
--如果报错 ORA-12712: new character set must be a superset of old character set 
--们的字符集：新字符集必须为旧字符集的超集，这时我们可以跳过超集的检查做更改： 
--SQL> ALTER DATABASE character set INTERNAL_USE ZHS16GBK; 

select * from v$nls_parameters; 
shutdown immediate; 
 startup 
  select * from v$nls_parameters; 
```

