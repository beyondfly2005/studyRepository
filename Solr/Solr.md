### 搜索实现方案的分析

#### 1、SQL模糊查询实现搜索

​	模块查询select * from t_product where name like '%关键词%'

​	**存在的问题**

##### **导致索引失效**

​	没有索引时，搜索会导致全表扫描

​	为提高索引的速度 为数据库表添加索引，类似书籍的目录，本质是一种数据结构

​	1>、哈希数据结构：方便定值数据查找

​	2>、树形数据结构：方便范围查找、提高搜索速度

​	3>、增加索引导致的副作用：更新操作会变慢，比起没有索引 要多维护了这棵树

​	4>、like ‘%A%’ 会导致索引失效，没有起始参照，会走权标扫描

​			like ‘A%’ 这样可以走索引，以A为参照，但是不能实现全文搜索 模糊查询等 不符合业务需求

​	BTREE索引结构 

##### 无法实现 自动分词搜索

​	比如“Nginx启动失败” 应该 匹配 Nginx-启动 -失败 三个关键词

#### 2、Mysql在5.6之后提供了一个全文索引

​	不使用MySQL全文索引的原因，是因为减轻数据库压力，让数据库少干点活；

​	走的是磁盘IO方式，不方面扩展；

#### 3、成熟的搜索引擎技术来踢打数据库搜索

- solr  Apache产品

- ES(elasticSearche )大数据分析

  适用范围：站内搜索


### 安装Solr服务器

#### 1、什么是Solr

Solr是用Java编写、运行在Servlet容器（Apache Tomcat 或Jetty）的一个独立的全文搜索服务器。Solr采用了Lucene Java 搜索库为核心的全文索引和搜索，并具有类似REST的HTTP/XML和Json的API。Solr强大的外部配置功能使得无需进行Java编码，便可以对其进行调整以适应多种类型的应用程序。

#### 2、Solr的运行环境

以 版本 Solr 4.10.4为例

JDK 1.7 以上
Servlet容器：Tomcat
Java平台：Windows或Linux

#### 3、下载JDK

```html
https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
```

#### 4、安装JDK

```bash
cd /usr/local/jdk
#解压jdk压缩包
tar -zxvf jdk
vim /etc/profile
#增加环境变量JAVA_HOME 修改PATH
source /etc/profile
# 验证
java -version 
```

#### 5、安装tomcat

```bash
cd /usr/local/tomcat
#解压
tar -zxvf tomcat.tar.gz
```

#### 6、下载Solr

```bash
https://lucene.apache.org/solr/downloads.html
##全部版本
http://archive.apache.org/dist/lucene/solr/

# 7.7.3/8.5.2版本可以从清华大学网站下载
https://mirrors.tuna.tsinghua.edu.cn/apache/lucene/solr/
```

#### 7、Solr安装 及目录介绍

```bash
# 上传solr压缩包 解压
tar -zxvf solr-4.10.4.tgz

# 目录介绍
bin：soklr的运行脚本
dist：包含一个可以连通tomcat和solrhome的可运行的war包
docs：solr的API文档
example：solr工程的示例目录
example/solr：该目录是一个标准的SolrHome，它包含一个默认的SolrCoare
```

#### 8、创建SolrHome(SolrCore)

```bash
SolrHome 可以包含多个SolrCore
cd example
cp -r solr /usr/local/solrhome
cd /usr/local
cp apache-tomcat-7.0.99.tar.gz /usr/local/solr
cd /usr/local/solr
tar -zxvf apache-tomcat-7.0.99.tar.gz
mv apache-tomcat-7.0.99 tomcat7
# rm -rf apache-tomcat-7.0.99.tar.gz
cd dist
cp solr-4.10.4.war /usr/local/tomcat7/webapps/
# 启动tomcat
cd ../bin
./startup.sh
tail -f ../logs/catalina.out
cd ../webapps
ll
mv solr-4.10.4 solr
rm -rf solr-.10.4.war


cd solr
ll
# 增加日志
cd WEB-INF/lib
cd WEB_INF
mkdir classes #用于存放log4j配置文件

cd /usr/loca/solr/sor-4.10.4/
# 将sor-4.10.4/example/lib/ext目录下的所有jar包复制到soolr工程的WEB-INF/lib目录下
cd example/lib/ext
ll
cp -r * /usr/local/solr/tomcat7/webapps/solr/WEB-INF/libs/
# 将log4j配置文件拷贝到classes中
cd /usr/loca/solr/sor-4.10.4/example/resources/
cp log4j.properties  /usr/local/solr/tomcat7/webapps/solr/WEB-INF/classes

#在solr应用中的web.xml文件中 加载SolrHome
cd /usr/local/solr/tomcat7/webapps/solr/WEB-INF/

vim web.xml

# 增加如下内容：
```

```xml
# 取消文档的注释
<env-entry>
    <env-entry-name>sor/home</env-entry-name>
    <evn-entry-value>/put/your/sor/home/here</evn-entry-value>
    <env-entry-type>java.lang.String</env-entry-type>
</env-entry>

# solrhome 目录 # /usr/local/sorhome

<env-entry>
    <env-entry-name>sor/home</env-entry-name>
    <evn-entry-value>/usr/local/sorhome</evn-entry-value>
    <env-entry-type>java.lang.String</env-entry-type>
</env-entry>

```

```bash
# 启动tomcat
cd /usr/local/solr/tomcat7/bin
./startup.sh
tail -f ../logs/catalina.out

# 验证安装是否成功
http://192.168.198.128:8080/solr
```

#### 9、 扩展中文分词器

IKAnalyzer分词器

- 将jar包上传到服务器

- 将IKAnalyzer依赖的jar包添加到slor工程中

  将dist/IKAnalyzer2012FF_u1.jar 上传到 /usr/local/solr/tomcat7/webapps/slor/WEB-INF/lib

- 将分析器使用额配置文件添加到classes中

  将ILAnalyzer.cfg.xml  和 stopword.dic 上传到/usr/local/solr/tomcat7/webapps/slor/WEB-INF/classes

- 修改schema.xml文件增加如下内容
```bash
cd /usr/local/solrhome/collection1/
cd conf
vim schema.xml
```

```xml
<!--IKAnalyzer-->
<fildType name="text_ik" class="solr.TextField">
    <analyzer class="org.wltea.analyzer.lucene.IKAnalyzer"/>
</fildType>

```

```bash
# 重启taomcat
cd /usr/local/solr/tomcat7/bin
./shutdown.sh
# 测试分词器作用
```

词典 停词 扩展新词 自定新词

```bash
cd /usr/local/solr/tomcat7/webapps/solr/WEB-INF/classes/
vim IKAnalyzer.cfg.xml
# 取消注释
# <entry key="ext_dict">ext.dic<entry>

vim ext.dic
# 增加 需要的新词 如 科比 
```



Mmseg4j 另外一款中文分词器

### SpringBOot整合Solr 操作solr数据

### 开发搜索系统，实现商品信息的搜索及展示

### 实现关键词高亮显示

### 实现搜索结果的分页效果

### Solr原理，为什么快

### Solr服务器多个solr-core的情况

### SolrCloud 多个服务器部署同一个soolr服务