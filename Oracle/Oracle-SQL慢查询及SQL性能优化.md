#### Oracle TOP SQL��ѯ
Oraclce Top��������
https://www.cnblogs.com/xibuhaohao/p/10253869.html
http://www.mamicode.com/info-detail-2581940.html


```sql
select t.SQL_ID,
       t.SERIAL#,
       t.USERNAME,
       t.SQL_ID,
       a.SQL_TEXT,
       a.SQL_FULLTEXT
  from v$session t, v$process s, v$sqlarea a
 where t.PADDR = s.ADDR
   and s.SPID in (��18348��)
   and a.SQL_ID = t.SQL_ID;
```

#### ��sql��ѯ

```sql
select *
 from (select SQL_ID, sa.SQL_TEXT,
        sa.SQL_FULLTEXT,
        sa.EXECUTIONS "ִ�д���",
        round(sa.ELAPSED_TIME / 1000000, 2) "��ִ��ʱ��",
        round(sa.ELAPSED_TIME / 1000000 / sa.EXECUTIONS, 2) "ƽ��ִ��ʱ��",
        sa.COMMAND_TYPE,
        sa.PARSING_USER_ID "�û�ID",
        u.username "�û���",
        sa.HASH_VALUE
     from v$sqlarea sa
     left join all_users u
      on sa.PARSING_USER_ID = u.user_id
     where sa.EXECUTIONS > 0
     order by (sa.ELAPSED_TIME / sa.EXECUTIONS) desc)
 where rownum <= 20;
 
 
 select *
 from (select SQL_ID, sa.SQL_TEXT,
        sa.SQL_FULLTEXT,
        sa.EXECUTIONS "ִ�д���",
        round(sa.ELAPSED_TIME / 1000000, 2) "��ִ��ʱ��",
        round(sa.ELAPSED_TIME / 1000000 / sa.EXECUTIONS, 2) "ƽ��ִ��ʱ��",
        sa.COMMAND_TYPE,
        sa.PARSING_USER_ID "91",
        u.username "ROOTEP",
        sa.HASH_VALUE
     from v$sqlarea sa
     left join all_users u
      on sa.PARSING_USER_ID = u.user_id
     where sa.EXECUTIONS > 0
     order by (sa.ELAPSED_TIME / sa.EXECUTIONS) desc)
 where rownum <= 20;
 
 select PARSING_USER_ID from v$sqlarea sa;
```

-- ORACLE ���� sql_id ��ѯ�󶨱����Ĵ���ֵ
--��ѯ��ǰ��ѯ��

```sql
select b.NAME,b.POSITION,b.DATATYPE_STRING,b.VALUE_STRING,b.LAST_CAPTURED
from v$sql_bind_capture b
where b.sql_id = 'du8u9yd5qqpnz';
```

--��ѯ��ʷ��ѯ��
```sql
select b.name, b.datatype_string, b.value_string, b.last_captured
from dba_hist_sqlbind b
where b.sql_id = 'XXXXXX'
order by 4;
```


#### Oracle connect by�Ż�

```sql
explain plan for select /*+ connect_by_filtering index(org)*/org.ID 
 FROM t_sys_organization org start with org.id=200000621
  connect by  prior  org.id = org.pid    ;
```

``` 
 explain plan for select /*+ index(org)*/org.ID 
 FROM t_sys_organization org start with org.id=200000621
  connect by  prior  org.id = org.pid    ;
```

```sql
select * from table(dbms_xplan.display);
```



#### Oracle�Ż������ܵ���

> https://blog.csdn.net/Aria_Miazzy/article/details/93204208

```sql
select t.SQL_ID,t.SERIAL#,t.USERNAME,t.SQL_ID,a.SQL_TEXT,a.SQL_FULLTEXT
  from v$session t, v$process s, v$sqlarea a
 where t.PADDR = s.ADDR and s.SPID in (��18348��) and a.SQL_ID = t.SQL_ID;
```


