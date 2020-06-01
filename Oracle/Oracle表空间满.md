### Oracle 表空间满处理方法

```sql

SELECT UPPER(F.TABLESPACE_NAME) "表空间名",D.TOT_GROOTTE_MB "表空间大小(M)",D.TOT_GROOTTE_MB - F.TOTAL_BYTES "已使用空间(M)",TO_CHAR(ROUND((D.TOT_GROOTTE_MB - F.TOTAL_BYTES) / D.TOT_GROOTTE_MB * 100,2),'990.99') "使用比",F.TOTAL_BYTES "空闲空间(M)",F.MAX_BYTES "最大块(M)"    
FROM (SELECT TABLESPACE_NAME,     
ROUND(SUM(BYTES) / (1024 * 1024), 2) TOTAL_BYTES,     
ROUND(MAX(BYTES) / (1024 * 1024), 2) MAX_BYTES     
FROM SYS.DBA_FREE_SPACE     
GROUP BY TABLESPACE_NAME) F,     
(SELECT DD.TABLESPACE_NAME,     
ROUND(SUM(DD.BYTES) / (1024 * 1024), 2) TOT_GROOTTE_MB     
FROM SYS.DBA_DATA_FILES DD     
GROUP BY DD.TABLESPACE_NAME) D     
WHERE D.TABLESPACE_NAME = F.TABLESPACE_NAME     
ORDER BY 4 DESC;


--查看表空间是否具有自动扩展的能力     
SELECT T.TABLESPACE_NAME,D.FILE_NAME,D.AUTOEXTENSIBLE,D.BYTES,D.MAXBYTES,D.STATUS     
FROM DBA_TABLESPACES T,DBA_DATA_FILES D     
WHERE T.TABLESPACE_NAME =D.TABLESPACE_NAME     
ORDER BY TABLESPACE_NAME,FILE_NAME; 
 

--给文件赋予增长权限
alter database datafile 'D:/Program Files/ORACLE/orcl/USERS01.DBF' autoextend on maxsize unlimited;

alter database datafile 'D:/Program Files/ORACLE/orcl/USERS01.DBF' autoextend on maxsize unlimited;


--新增db文件
Alter tablespace USERS add datafile 'D:/Program Files/ORACLE/orcl/USERS02.DBF' size 1024M autoextend on next 1024M Maxsize UNLIMITED;
```

参考：https://blog.csdn.net/qq_35257875/article/details/90295272