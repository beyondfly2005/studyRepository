# 一 、工作流介绍

## 1.1 概念

工作流WorkFLow 就是通过计算机对业务流程自动哈执行管理。它主要解决的是“使在多个参与者之间安装某种预定义的规则自动进行传递文档、信息、或任务的过程”，从而实现预期的业务布标，或者促使此目标的实现。

## 1.2 工作流系统

一个软件系统中具有工作流的功能，我们把它称为工作流系统

## 1.3 适用行业

消费品行业，制造业、电信服务业、

## 1.4 具体应用



## 1.5 实现方式

在没有专门的工作流引擎之前，我们之前为了实现流程控制，通常的做法就是采用状态字段的值来跟踪流程的变化情况，这样不用角色的用户，通过状态字段的取值来决定记录是否显示。

针对有权限的

工作流引擎



# 二、Activiti7 概述

## 2.1 介绍

使用专门的建模语言BPMN2.0进行定义

官方网站 https://www.activiti.org/



### 2.1.1 BPM

BPM（Business Process Management） 业务流程管理

### 2.2.2 BPMN

事件 用圆形表示 绿色-开始 红色-结束

圆角矩形 表示

## 2.2 使用步骤



##### 部署activiti

##### 流程定义

##### 使用定义部署

##### 启动一个流程实例

##### 用户查询待办任务Task

##### 用户办理任务

##### 流程结束



# 三、Activiti环境

## 3.1 开发环境

Jdk1.8

Mysql5及以上版本

Tomcat 8.5及以上

IDEA开发工具

## 3.2  Activiti环境

我们使用Activiti7.0.0.Beta1默认支持Spring5

#### 3.2.1  下载Activiti7

Maven 依赖如下

```xml
<dependency>
	<groupId>org.activiti</groupId>
    <artifactId>activiti-denpendencies</artifactId>
    <version></version>
</dependency>
```

数据库支持

运行activiti 需要有数据的支持，支持的数据库有H2 mysql oracle postres  MS-SQL DB2



#### 3.2.2 流程设计器IDEA的安装

actiBMP插件



## 3.3 Activiti的数据库支持

#### 3.3.1Activiti支持的数据库

| 数据库类型 | 版本                   | JDBC连接示例                    | 说明             |
| ---------- | ---------------------- | ------------------------------- | ---------------- |
| h2         | 1.3.168                | jdbc:h2:tcp//localhost/activiti | 默认配置的数据库 |
| mysql      | 5.1.21                 |                                 |                  |
| oracle     | 11.2.0.1.0             |                                 |                  |
| postgres   | 8.1                    |                                 |                  |
| db2        | DB2 10.1 using db2jcc4 |                                 |                  |
| MS-SQL     | 2008 using sqljdbc4    |                                 |                  |



### 3.3.2 在mysqk生成表

```sql
CREATE DATABASE activiti DEFAULT CHARACTER SET utf8;
```

#### 3.3.2.2 使用java代码生成表

##### 1) 创建java工程

使用idea创建java的maven工程 取名 activiti01

##### 2) 加入maven依赖的坐标（jar包）

首先需要在java工程中加入ProcessEngine所需要的jar包 包括

1 activiti-engine-7.0.0.beta1.jar

2 activiti 依赖的jar包 mybatis slf4j log4j

3 activiti 依赖的spring包

4 mysql数据库驱动

5 第三方数据连接池dbcp

6 单元测试工具junit-4.12.jar

我们用maven来实现项目的搭建，索引应导入这些jar对于的坐标到pom.xml文件中

完整的依赖内容如下：

```xml
<properties>
        <slf4j.version>1.6.6</slf4j.version>
        <log4j.version>1.2.12</log4j.version>
        <activiti.version>7.0.0.Beta1</activiti.version>
    </properties>

    <dependencies>
        <!-- activiti 的相关jar包 -->
        <dependency>
            <groupId>org.activiti</groupId>
            <artifactId>activiti-engine</artifactId>
            <version>${activiti.version}</version>
        </dependency>
        <dependency>
            <groupId>org.activiti</groupId>
            <artifactId>activiti-spring</artifactId>
            <version>${activiti.version}</version>
        </dependency>
        <!-- bpmn模型 -->
        <dependency>
            <groupId>org.activiti</groupId>
            <artifactId>activiti-bpmn-model</artifactId>
            <version>${activiti.version}</version>
        </dependency>
        <!-- bpmn转换 -->
        <dependency>
            <groupId>org.activiti</groupId>
            <artifactId>activiti-bpmn-converter</artifactId>
            <version>${activiti.version}</version>
        </dependency>
        <!-- bpmn json -->
        <dependency>
            <groupId>org.activiti</groupId>
            <artifactId>activiti-json-converter</artifactId>
            <version>${activiti.version}</version>
        </dependency>
        <!-- bpmn 布局-->
        <dependency>
            <groupId>org.activiti</groupId>
            <artifactId>activiti-bpmn-layout</artifactId>
            <version>7.0.0.Beta1</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.activiti</groupId>
            <artifactId>activiti-bpmn-layout</artifactId>
            <version>${activiti.version}</version>
        </dependency>
        <!-- activiti 云支持-->
        <dependency>
            <groupId>org.activiti.cloud</groupId>
            <artifactId>activiti-cloud-services-api</artifactId>
            <version>${activiti.version}</version>
        </dependency>

        <!--mysql 驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>

        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>
        <dependency>
            <groupId>commons-dbcp</groupId>
            <artifactId>commons-dbcp</artifactId>
            <version>1.4</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
    </dependencies>
```

###### log4j日志配置

sources 下创建log4j.properties文件

```properties
Set root category priority to INFO and its only appender to CONSOLE.
#log4j.rootCategory=INFO, CONSOLE debug info warn error fatal
log4j.rootCategory=debug, CONSOLE, LOGFILE

# Set the enterprise logger category to FATAL and its only appender to CONSOLE.
log4j.logger.org.apache.axis.enterprise=FATAL, CONSOLE

# CONSOLE is set to be a ConsoleAppender using a PatternLayout.
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m

# LOGFILE is set to be a File appender using a PatternLayout.
log4j.appender.LOGFILE=org.apache.log4j.FileAppender
log4j.appender.LOGFILE.File=/Users/apple/学习/study/activity/activity_01/xis.log
log4j.appender.LOGFILE.Append=true
log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.LOGFILE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m     

```

###### activit配置文件

我们使用activiti提供的默认方式来创建mysql的表。

默认方式的要求是在resources下创建activiti.cfg.xml文件，注意：默认方式目录和文件名不能修改，因为activiti的源码中以及设置，到股东的目录读取固定文件名的文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
						http://www.springframework.org/schema/contex http://www.springframework.org/schema/context/spring-context.xsd
						http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
</beans>						
```

###### 在activiti.cfg.xml中进行配置

默认方式要在activiti.cfg.xml中的bean的名字叫processEngineConfiguration，名字不可修改。

在这里有两种配置方式：一种是单独配置数据源，一种是不单独配置数据源

1、直接配置processEngineConfiguration

```xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
						http://www.springframework.org/schema/contex http://www.springframework.org/schema/context/spring-context.xsd
						http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
 
    <!-- 默认方式下：bean的id=processEngineConfiguration 固定不能改变 -->
 
    <bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
		<!-- 配置数据库相关的信息 -->
        <!-- 配置数据库驱动 -->
        <property name="jdbcDriver" value="com.mysql.jdbc.Driver" />
        <!-- 配置数据库链接 -->
	    <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/activiti" />
        <!-- 配置数据库用户名 -->
    	<property name="jdbcUsername" value="root" />
        <!-- 配置数据库密码-->
	    <property name="jdbcPassword" value="root" />
        <!-- activiti数据库表在生成时的策略 true 如果存在则直接使用，如果不存在则创建-->
        <property name="databaseSchemaUpdate" value="true"/>
    </bean>
</beans>  
```

java测试类，生产activiti相关表

```java
package com.beyondsoft.test;

import org.activiti.engine.ProcessEngine;
import org.activiti.engine.ProcessEngines;
import org.junit.Test;

public class TestCreate {

    //使用默认方式创建mysql相关的表

    @Test
    public void testCreateDbTable(){
        //使用activiti提供的工具类
        //创建processEngine的时候，就会自动创建表
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
        System.out.println(processEngine);
    }
}

```

## 3.4 表结构介绍

### 3.4.1 表的命名规则和作用

activiti会自动创建25张表

ge - gennel 通用 通用数据表

hi - history 历史

re - repository 库

ru - runtime 运行时

### 3.4.1 Activiti数据表介绍

| 表分类       | 表名                | 解释                           |
| ------------ | ------------------- | ------------------------------ |
| 一般数据     | act_ge_bytearray    | 通用的流程定义和流程资源       |
|              | act_ge_property     | 系统相关属性                   |
| 流程历史记录 |                     |                                |
|              | act_hi_actinst      | 历史节点表                     |
|              | act_hi_attachment   | 历史附件表                     |
|              | act_hi_comment      | 历史意见表                     |
|              | act_hi_detail       | 历史详情表，提供历史变量的查询 |
|              | act_hi_detail       | 历史流程用户信息表             |
|              | act_hi_procinst     | 历史流程实例表                 |
|              | act_hi_taskinst     | 历史任务实例表                 |
|              | act_hi_varinst      | 历史变量表                     |
| 流程定义表   |                     |                                |
|              | act_re_deployment   | 部署信息表                     |
|              | act_re_model        | 流程设计模型部署表             |
|              | act_re_procdef      | 流程定义数据表                 |
| 运行实例表   |                     |                                |
|              | act_ru_event_subscr | 运行时事件表                   |
|              | act_ru_execution    | 运行时流程执行实例表           |
|              | act_ru_identitylink | 运行时用户信息表               |
|              | act_ru_job          | 运行时作业信息表               |
|              | act_ru_task         | 运行时任务信息表               |
|              | act_ru_variable     | 运行时变量信息表               |
| 其他表       |                     |                                |
|              | act_evt_log         | 流程引擎的通用事件日志记录表   |
|              | act_procdef_info    | 流程定义的动态变更信息         |

# 四、Activiti类关系图

## 4.1 Activiti类关系图

![img](https://img-blog.csdnimg.cn/20191226173208831.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9jd2wtamF2YS5ibG9nLmNzZG4ubmV0,size_16,color_FFFFFF,t_70)

在新版本中，我们通过实验可以发现 IdentityService，FormService 两个 Serivce 都已经删除了。所以后面我们对于这两个 Service 也不讲解了，但老版本中还是有这两个 Service，同学们需要了解一下。

## 4.2 activiti.cfg.xml

activiti 的引擎配置文件，包括：`ProcessEngineConfiguration` 的定义、数据源定义、事务管理器等，此文件其实就是一个 spring 配置文件

## 4.3 流程引擎配置类

流程引擎的配置类，通过 ProcessEngineConfiguration 可以创建工作流引擎 ProceccEngine，常用的两种方法如下：

### 4.3.1 StandaloneProcessEngineConfiguration

通过 `org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration`

Activiti 可以单独运行，使用它创建的 ProcessEngine，Activiti 会自己处理事务。

配置文件方式：
通常在 activiti.cfg.xml 配置文件中定义一个 id 为 processEngineConfiguration 的 bean，这里会使用 spring 的依赖注入来构建引擎。
方法如下：

第一种方式 使用

```xml
<bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
	<!-- 数据源 --> 
	<property name="dataSource" ref="dataSource" />
	<!-- 数据库策略 --> 
	<property name="databaseSchemaUpdate" value="true"/>
</bean>
```

第二种方式 使用连接池

```xml
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/activiti"/>
    <property name="username" value="root"/>
    <property name="password" value="123456"/>
    <property name="maxActive" value="3"/>
    <property name="maxIdle" value="1"/>
</bean>

<bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
	<!--数据源 直接引用上面哦诶之的连接池 --> 
	<property name="dataSource" ref="dataSource" />
	<!-- 数据库策略 --> 
	<property name="databaseSchemaUpdate" value="true"/>
</bean>
```



### 4.3.2 SpringProcessEngineConfiguration

通过 `org.activiti.spring.SpringProcessEngineConfiguration` 与 Spring 整合。

创建 spring 与 activiti 的整合配置文件：
`activity-spring.cfg.xml`（名称可修改 不固定）

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-3.1.xsd
http://www.springframework.org/schema/mvc
http://www.springframework.org/schema/mvc/spring-mvc-3.1.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context-3.1.xsd
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop-3.1.xsd
http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx-3.1.xsd ">

    <!-- 工作流引擎配置bean -->
    <bean id="processEngineConfiguration" class="org.activiti.spring.SpringProcessEngineConfiguration">
        <!-- 数据源 -->
        <property name="dataSource" ref="dataSource" />
        <!-- 使用spring事务管理器 -->
        <property name="transactionManager" ref="transactionManager" />
        <!-- 数据库策略 -->
        <property name="databaseSchemaUpdate" value="drop-create" />
        <!-- activiti的定时任务关闭 -->
        <property name="jobExecutorActivate" value="false" />
    </bean>

    <!-- 流程引擎 -->
    <bean id="processEngine" class="org.activiti.spring.ProcessEngineFactoryBean">
        <property name="processEngineConfiguration" ref="processEngineConfiguration" />
    </bean>

    <!-- 资源服务service -->
    <bean id="repositoryService" factory-bean="processEngine" factory-method="getRepositoryService" />

    <!-- 流程运行service -->
    <bean id="runtimeService" factory-bean="processEngine" factory-method="getRuntimeService" />

    <!-- 任务管理service -->
    <bean id="taskService" factory-bean="processEngine" factory-method="getTaskService" />

    <!-- 历史管理service -->
    <bean id="historyService" factory-bean="processEngine" factory-method="getHistoryService" />

    <!-- 用户管理service -->
    <bean id="identityService" factory-bean="processEngine" factory-method="getIdentityService" />

    <!-- 引擎管理service -->
    <bean id="managementService" factory-bean="processEngine" factory-method="getManagementService" />

    <!-- 数据源 -->
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver" />
        <property name="url" value="jdbc:mysql://localhost:3306/activiti" />
        <property name="username" value="root" />
        <property name="password" value="mysql" />
        <property name="maxActive" value="3" />
        <property name="maxIdle" value="1" />
    </bean>

    <!-- 事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource" />
    </bean>

    <!-- 通知 -->
    <tx:advice id="txAdvice" transaction-manager="transactionManager"> <tx:attributes>

        <!-- 传播行为 -->
        <tx:method name="save*" propagation="REQUIRED" />
            <tx:method name="insert*" propagation="REQUIRED" />
            <tx:method name="delete*" propagation="REQUIRED" />
            <tx:method name="update*" propagation="REQUIRED" />
            <tx:method name="find*" propagation="SUPPORTS" read-only="true" />
            <tx:method name="get*" propagation="SUPPORTS" read-only="true" />
        </tx:attributes>
    </tx:advice>
    
    <!-- 切面，根据具体项目修改切点配置 --> 
    <aop:config proxy-target-class="true"> 
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.itheima.ihrm.service.impl.*.*(..))" />
    </aop:config>
</beans>
```

## 4.4 工作流引擎创建

工作流引擎，相当于一个门面接口，通过 `ProcessEngineConfiguration` 创建 `processEngine`，通过`ProcessEngine` 创建各个 service 接口。

### 4.4.1简单创建方式（默认创建方式）

将 `activiti.cfg.xml` 文件名及路径固定，且 `activiti.cfg.xml` 文件中有 `processEngineConfiguration` 的配置，可以使用如下代码创建 `processEngine`:

```java
//使用classpath下的activiti.cfg.xml中的配置创建processEngine
ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
System.out.println(processEngine);
```
### 4.4.1 一般创建方式（自定义创建方式）

```java
//先构建ProcessEngineConfiguration
//ProcessEngineConfiguration processEngineConfiguration = ProcessEngineConfiguration.createProcessEngineConfigurationFromResource("activiti.cfg.xml");
//bean的名字可以自定义
ProcessEngineConfiguration processEngineConfiguration = ProcessEngineConfiguration.createProcessEngineConfigurationFromResource("activiti.cfg.xml","processEngineConfiguration");
//通过ProcessEngineConfiguration创建ProcessEngine
ProcessEngine processEngine = processEngineConfiguration.buildProcessEngine();
System.out.println(processEngine);
```

## 4.5 Service服务接口

### 4.5.1 Service 创建方式

通过 ProcessEngine 创建 Service，Service 是工作流引擎提供用于进行工作流部署、执行、管理的服务接口。我们使用这些接口就可以操作服务对应的数据库表。

方式如下：

```java
RuntimeService runtimeService = processEngine.getRuntimeService();
RepositoryService repositoryService = processEngine.getRepositoryService();
TaskService taskService = processEngine.getTaskService();
```

### 4.5.2 Service 总览

| Service           | 类别                      |
| ----------------- | ------------------------- |
| RepositoryService | activiti 的资源管理类     |
| RuntimeService    | activiti 的流程运行管理类 |
| TaskService       | activiti 的任务管理类     |
| HistoryService    | activiti 的历史管理类     |
| ManagerService    | activiti 的引擎管理类     |

### 4.5.3 RepositoryService

是 activiti 的资源管理类，提供了管理和控制流程发布包和流程定义的操作。使用工作流建模工具设计的业务流程图需要使用此 service 将流程定义文件的内容部署到计算机。

除了部署流程定义以外还可以：

查询引擎中的发布包和流程定义。

暂停或激活发布包，对应全部和特定流程定义。 暂停意味着它们不能再执行任何操作了，激活是对应的反向操作。

获得多种资源，像是包含在发布包里的文件， 或引擎自动生成的流程图。

获得流程定义的 pojo 版本， 可以用来通过 java 解析流程，而不必通过 xml。

### 4.5.4 RuntimeService

它是 activiti 的流程运行管理类。可以从这个服务类中获取很多关于流程执行相关的信息

### 4.5.5 TaskService

是 activiti 的任务管理类。可以从这个类中获取任务的信息。

### 4.5.6 HistoryService

是 activiti 的历史管理类，可以查询历史信息，执行流程时，引擎会保存很多数据（根据配置），比如流程实例启动时间，任务的参与者， 完成任务的时间，每个流程实例的执行路径，等等。 这个服务主要通过查询功能来获得这些数据。

### 4.5.7 ManagementService

是 activiti 的引擎管理类，提供了对 Activiti 流程引擎的管理和维护功能，这些功能不在工作流驱动的应用程序中使用，主要用于 Activiti 系统的日常维护。



# 五、Activiti入门操作

在本章内容中，我们来创建一个Activiti工作流，并启动这个流程。

创建Activiti工作流主要包含以下几步：

1、定义流程： 按照BPMN规范，使用流程定义工具，永**流程符号**把整个流程描述出来

2、部署流程： 把画好的流程定义文件，加载到数据库中，生成表的数据

3、启动流程： 使用java代码来操作数据库表中的内容

## 5.1 流程符号

BPMN 2.0 是业务流程建模符号2.0的缩写。

它由Business Process Management Initiative这个非营利协会创建并不断发展。作为一种标识，BPMN 2.0是使用一些符号来明确业务流程设计流程图的一整套符号规范，它能增进业务建模时的沟通效率。

目前BPMN2.0是最新的版本，它用于在BPM上下文中进行布局和可视化的沟通。

接下来我们先来了解在流程设计中常见的符号。

BPMN2.0的**基本符号**主要包括：

#### 事件Event

Start Event开始事件 Internediate Event中间事件 EndEvent结束事件

#### 活动Activity

User Task用户任务 Service Task服务任务 Sub Process 子流程

#### 网关GateWay

排他网关 并行网关 包容网关 综合网关 事件网关

#### 流向Flow

顺序流Sequence Flow

消息流MessageFlow

关联Association

数据关联DataAssociation

## 5.2 流程设计器的使用

#### Activiti-Designer使用

##### Palette（画板）



##### 新建流程（IDEA工具）





可能存在的问题

右键找不到Diagrams

https://blog.csdn.net/wk52525/article/details/79362904

xml中文乱码问题



# 六、流程操作

## 6.1 流程定义

## 6.2 流程定义部署

## 6.3 启动流程实例

## 6.4 任务查询

## 6.5 流程任务处理

## 6.6 流程定义信息查询

查询流程相关信息，包含流程定义，流程部署，流程定义版本

```java
//查询流程定义
    @Test
    public void queryProcessDefinition(){
        //1、获取流程引擎processEngine
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
        //2、获取RepositoryService
        RepositoryService repositoryService =processEngine.getRepositoryService();
        //获取ProcessDefinitionQuery对象
        ProcessDefinitionQuery processDefinitionQuery = repositoryService.createProcessDefinitionQuery();
        //查询当前所有的流程定义
        List<ProcessDefinition> definitionList = processDefinitionQuery.processDefinitionKey("myEvection").orderByProcessDefinitionVersion().desc().list();
        //输出信息
        for (ProcessDefinition processDefinition : definitionList) {
            System.out.println("流程定义id="+processDefinition.getId());
            System.out.println("流程定义名称="+processDefinition.getName());
            System.out.println("流程定义key="+processDefinition.getKey());
            System.out.println("流程定义版本="+processDefinition.getVersion());
            System.out.println("流程部署id="+processDefinition.getDeploymentId());
        }
    }
```



## 6.7 流程删除

```java
    //删除流程部署信息
    @Test
    public void deleteDeployment(){
        //1、获取流程引擎processEngine
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
        //2、获取RepositoryService
        RepositoryService repositoryService = processEngine.getRepositoryService();
        //通过流程部署id
        String deploymentId ="1";
        repositoryService.deleteDeployment(deploymentId);
    }
```

删除的表包括

```java
act_ge_bytearray
act_re_deployment
act_re_procdef

历史信息不删除
act_his

未完成的流程
默认不允许删除 如果想要删除 需要使用级联删除
repositoryService.deleteDeployment(deploymentId,true); //级联删除 未完成的任务也一起删除 cascade=true

```

## 6.8 流程资源下载

我们的流程资源已经上传到数据库了，如果其他用户想要查看这些资源文件，可以从数据库中把资源文件下载到本地

解决方案有：

1、jdbc对blob类型 clob类型数据读取出来，保存到文件目录

2、使用activiti的api来实现

使用commons-io.jar解决IO的操作

引入commons-io依赖包

```xml
<dependency>
	<groupId>common-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
```

通过流程定义对象获取流程定义资源，获取bpmn和png文件

```java

```



## 6.9 流程历史信息的查看