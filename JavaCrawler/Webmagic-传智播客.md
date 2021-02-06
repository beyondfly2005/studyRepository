> https://www.bilibili.com/video/BV1kV411d7zH?p=2

> 传智播客 爬虫 Webmagic

### 1 谈谈网络爬虫

#### 1.1 什么是网络爬虫

#### 1.2 网络爬虫可以做什么

#### 1.3 网络爬虫常用的技术

##### 1.3.1 底层实现HttpClient + JSOUP

##### 1.3.2 开源框架WebMagic



### 2 爬虫框架Webmagic

#### 2.1 架构解析

四大组件

- Downloader

负责从互联网下载页面，以便后续出来，WebMagic默认使用

- PageProcessor
- Scheduler
- Pipline

#### 2.2 PageProcessor

##### 2.2.1 爬取页面全部内容

需求 编写爬虫程序，趴下csdn中博客的内容 http://blog.csdn.net

(1) 创建工程，引入依赖

```xml
<dependency>
	<groupId>us.codecraft</groupId>
    <artifactId>webmagic-core</artifactId>
    <version>0.7.3</version>
</dependency>
<dependency>
	<groupId>us.codecraft</groupId>
    <artifactId>webmagic-extension</artifactId>
    <version>0.7.3</version>
</dependency>
```

(2) 编写 类

```java
package com.beyondsoft.webmagic;

import us.codecraft.webmagic.Page;
import us.codecraft.webmagic.Site;
import us.codecraft.webmagic.Spider;
import us.codecraft.webmagic.processor.PageProcessor;

/**
 爬取类
 */
public class MyProcessor implements PageProcessor {
    public void process(Page page){
        //打印页面内容
        System.out.println(page.getHtml().toString());
    }

    public Site getSite(){
        return Site.me().setSleepTime(100) //设置休眠时间 防止网站有防爬虫
                .setRetryTimes(3);
    }

    public static void main(String[] args){
        Spider.create(new MyProcessor()).addUrl("http://blog.csdn.net");
    }
}
```

(3) 调用



##### 2.2.2 爬取指定内容（XPath）

如果我们需要爬取页面中的部分内容，需要指定XPath。

XPath，纪委XML路径语言（XML PathLanguage），它是一种用来确定XML文档中某部分位置的语言，Xpath使用路径表达式来选取XML文档中的节点或者节点集。这些路径表达式和我们在常规的电脑文件系统中看到的表达式非常相似，语法详解附录A

我们通过指定xpath来抓取网页的部分内容

```

```

##### 附录 XPath语法

（1）选取节点

| 表达式   | 描述                                                       |
| -------- | ---------------------------------------------------------- |
| ndoename | 选取此节点的所有子节点                                     |
| /        | 从根节点选取                                               |
| //       | 从匹配选择的当前节点选择文档中的节点，而不考虑他们的位置。 |
| .        | 选取当前节点                                               |
| ..       | 选取当前节点的父节点                                       |
| @        | 选取属性                                                   |

（2） 谓语

谓语用来查找某个特定的节点或者包含某个指定的值的节点。

谓语被嵌在方括号中

在下面的表格中，我们列出了带有谓语的一些路径表达式，以及表达式的结果：

| 表达式                              | 含义                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| /bookstore/book[1]                  | 选取属于bookstore子元素的第一个book元素                      |
| /bookstore/book[last()]             | 选取属于 bookstore 子元素的最后一个 book 元素。              |
| /bookstore/book[last()-1]           | 选取属于 bookstore 子元素的倒数第二个 book 元素。            |
| /bookstore/book[position()<3]       | 选取最前面的两个属于 bookstore 元素的子元素的 book 元素。    |
| //title[@lang]                      | 选取所有拥有名为 lang 的属性的 title 元素。                  |
| //title[@lang='eng']                | 选取所有 title 元素，且这些元素拥有值为 eng 的 lang 属性。   |
| /bookstore/book[price>35.00]        | 选取 bookstore 元素的所有 book 元素，且其中的 price 元素的值须大于 35.00。 |
| /bookstore/book[price>35.00]//title | 选取 bookstore 元素中的 book 元素的所有 title 元素，且其中的 price 元素的值须大于 35.00。 |

(3)  通配符

| 用法   | 说明                 |
| ------ | -------------------- |
| *      | 匹配任何元素节点。   |
| @*     | 匹配任何属性节点。   |
| node() | 匹配任何类型的节点。 |

（4）Xpath轴

（5）Xpath运算符

（6）

（7）常用的功能函数

| 表达式 | 描述 | 用法 |
| ------ | ---- | ---- |
|        |      |      |



##### 2.2.3 添加目标地址

我们可以通过添加目标地址，从种子页面爬到更多的页面

```java
public void process(Page page){
    page.addTargetRequests(page.getHtml().links().all()); //将当前页面里的所有链接都添加到目标页面中
    System.out.println(page.getHtml().xpath("//[@id=\"nav\"]/div/div/ul/li[5]/a").toString());
}
```



##### 2.2.4 目标地址正则匹配

#### 2.3 Pipeline

##### 2.3.1 ConsolePipeline 控制台输出

```java

```



##### 2.3.2 FilePipeline 文件输出

##### 2.3.3 JsonFilePipeline Json文件输出

##### 2.3.4 定制Pipeline

如果以下Pipline 不能满足，你可以定制

```java
package com.beyondsoft.webmagic;

import us.codecraft.webmagic.ResultItems;
import us.codecraft.webmagic.Task;
import us.codecraft.webmagic.pipeline.Pipeline;

public class MyPipeline implements Pipeline {

    public void process(ResultItems resultItems, Task task) {
        String title = resultItems.get("title").toString();
        System.out.println("定制Pipeline title:"+title);
    }
}
```

调用

```java
    public static void main(String[] args){
        Spider.create(new MyProcessor())
                .addUrl("http://blog.csdn.net")               
                .addPipeline(new MyPipeline())
                .run();
    }
```



#### 2.4 Scheduler

Scheduler(URL管理)最基本的功能是实现对已经爬取的URL进行标识，可以实现URL的增量去重。

**目前Scheduler主要有三种实现方式：**

- 内存队列 QueueScheduler
- 文件队列 FileCacheQueueScheduler
- Redis队列 RedisScheduler

##### 2.4.1 内存队列

```java
    public static void main(String[] args){
        Spider.create(new MyProcessor())
                .addUrl("http://blog.csdn.net")               
                .setScheduler(new QueueScheduler())
                .run();
    }
```

内存方式存在的问题：只保证本次爬取不重复，下次在对这个网站进行爬取可能会重复

##### 2.4.2 文件队列

```java
    public static void main(String[] args){
        Spider.create(new MyProcessor())
                .addUrl("http://blog.csdn.net")               
                .setScheduler(new QueueScheduler("e://scheduler"))
                .run();
    }
```

文件方式可以解决多次爬取同一网站重复问题，但是如果做的是一个分布式爬虫 无法解决同一网站的重复问题 ，适合单机爬虫

##### 2.4.3 Redis队列

```java
    public static void main(String[] args){
        Spider.create(new MyProcessor())
                .addUrl("http://blog.csdn.net")               
                .setScheduler(new RedisScheduler())
                .run();
    }
```



### 3 十次方文章数据爬取

#### 3.1 需求分析

每天某时间段从CSDN博客中爬取文档，存入文章数据库中。

#### 3.2 频道设置

| 频道名称 | 地址                                |
| -------- | ----------------------------------- |
| 资讯     | https://www.csdn.net/nav/news       |
| 人工智能 | https://www.csdn.net/nav/ai         |
| 区块链   | https://www.csdn.net/nav/blockchain |
| 前端     | https://www.csdn.net/nav/web        |
| 编程语言 | https://www.csdn.net/nav/lang       |
| Java     | https://www.csdn.net/nav/java       |

数据库tensquare_article表tb_channel表

| id   | name     | state |
| ---- | -------- | ----- |
| news | 咨询     | 1     |
| AI   | 人工智能 | 1     |
| web  | 前端     |       |
| lang | 编程语言 |       |

(1) POM依赖

- 爬虫
- springboot-jpa



(2) 配置文件



(3) 启动类

```java
public class ArticleCrawlerApplication {

    public static void manin(String[] args){
        
    }
}
```

(4) 爬虫启动类

#### 3.3 代码编写

##### 3.3.1 模块搭建

##### 3.3.2 爬取类

##### 3.3.3 入库类

##### 3.3.4 任务类

### 4. 十次方用户数据爬取
#### 4.1 需求分析
#### 4.2 代码编写
##### 4.2.1 模块搭建
##### 4.2.2 爬取类

##### 4.2.3 下载工具类

##### 4.2.4 入库类

##### 4.2.5 任务类