> https://www.bilibili.com/video/BV1Xb411q7h6?p=61

SQL优化

1、对查询进行优化，尽量避免全表扫描，首先应考虑where 及order by 设计的列上建立索引、

2、应尽量避免在where字句中对字段进行null值判断，否则将导致引擎放弃索引而进行全表扫描

如：select id from t where num is null

可以在num上设置默认值为0 确保num列没有null值

然后改为这样

select id from t where num=0

3、应尽量避免在where字句中使用!=或<>操作符，否则引擎将放弃使用索引而引起全表扫描

4、应尽量避免在where字句中使用 or 来连接条件，否则将导致引擎方式使用索引而进行全表扫描

例如select id from t where num=10 or num=20

解决方案

select id form t where num=10

union all

select id form t where num20

3、in和not in 也要慎用否则会导致全表扫描

select id from t where num in(1,2,3)

对于连续的值，能用berween and就不用用in

select id from t where num between 1 and 3;



6、双侧like通配查询也将导致全表扫描

select id from t where name like '%abc%';



7、应尽量避免在where字句中进行字段进行表达式操作，浙江导致引擎放肆使用索引而进行全表扫描

如：

select id from t where num/2=100;

应改为

select id from t where num=100*2;

8、应尽量避免在where字句中对字段进行函数操作，浙江导致引擎方式使用索引而进行全表扫描

如

select id from t where substring(name,1,3)='abc'   --以abc开头的id

应该改为

select id from t  where name like 'abc%'

9、不用再where字句中的=坐标进行函数、算术运算符或其他表达式运算，否则系统将可能无法正确使用索引

10、在使用索引自动作为条件时，如果该索引是复合索引，那么必须使用该所与中的第一个字段作为调节是才能保证系统使用该索引，否则该所与将不会被使用，并且应尽可能的让自动顺序与索引顺序向一致。

11、不要写一些没有意义的查询，如生产一个空表结构

select col1,col2,col3 into #t where 1=2

这里代码不会反悔任何结果集，但是会消耗系统资源，应该改成这样：

create table #t( ... ... );

12、很多时候使用exists代替in是一个很好的选择

select num from a where num in(select num from b)

用下面的语句替换

select num from a where exists(select 1 from b where num=a.num)

13、并不是所有所有对查询都有效，SQL是根据表中数据来进行查询欧化的，当所有列有大量数据重复是，SQL查询可能不会去利用索引，如一表中有自动sex male female 几乎各一半，那么即使在sex中建立索引也对查询效率起不到作用

不重复的 唯一的 查询效率最快

14、索引并不是越多越好，索引固然可以提高相应的select 查询效率，但也同时降低了insert及update的效率，因为在insert或update是有坑呢会重建索引，所以怎样建立索引需要慎重考虑，试具体情况而定，一个表的索引最后不要超过6个， 若太多则应考虑一些补偿使用到的列上建立索引是否有必要。

15、尽量使用数字型字段，若只含数值信息的字段尽量不要设计为字符型，这样会降低查询和连接的性能，炳辉增加存储开销，这是因为引擎在处理查询和连接时，会逐个比较字符中每一个字符，而对于数字型而言只需要比较一次就够了

16、尽可能的使用varchar代替char，因为首先可变长字段存储空间小，可以接受存储空间，其次对于查询来说，在一个相对较小的自动内搜索效率显然要高些。

*短索引*

17、任何地方都不要使用select * from t ，应该用具体列；来代替 * ，不要反悔用不到的任何字段。



SQL性能优化

1、先做SQL优化

2、建立索引优化

3、调整引擎优化









