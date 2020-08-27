#### 1、Mybatis中 #{parm}  ${parm} 的区别

- 在引用参数时使用的两种方式#{} ${}

- #{}把这个参数认为是一个字符串，并自动加上"" 引号 

- ${para} 不会加速'' 单引号

- **#{}是经过预编译的,是安全的**。

  而**${}**是未经过预编译的,**仅仅是取变量的值,是非安全的,存在SQL注入**。

- ${}一般用于order by的后面，，Mybatis不会对这个参数进行任何的处理，直接生成了sql语句 select * from 表名 order by ${age}

- 一般我们使用#{}，不使用${}，原因：
  会引起sql注入，${}会直接参与sql编译。会影响sql语句的预编译。

-  #方式能够很大程度防止sql注入。

- $方式无法防止Sql注入。

- $方式一般用于传入数据库对象，例如传入表名.

- 一般能用#的就别用$.

- MyBatis排序时使用order by 动态参数时需要注意，用$而不是#

  


#### 2、Mybatis 作用域（Scope）和生命周期

| 类名称                   | SCOPE                               |
| ------------------------ | ----------------------------------- |
| SqlSessionFactoryBuilder | method                              |
| SqlSessionFactory        | application                         |
| SqlSession               | request/method （可以认为是线程级） |
| Mapper                   | method                              |