> https://www.bilibili.com/video/BV12E411X77i?p=2

## 单体架构

1、 什么是单体架构

为什么要分层

- 隔离关注点
  - controller 完成与用户的交互：1-展示数据，2-收集数据
  - service：处理业务逻辑
  - 处理与数据库的交互

一个业务操作可能涉及多个dao操作

事务的边界：事务放到业务层，保证多个dao操作，要么都成功，要么都失败

各层技术选型

- Controller层
  - Struts2
  - SpringMVC
- Service层
  - Spring
- Dao层
  - Hibernate
  - MyBatis
  - JPA

单体架构的优缺点

- 部署简单
- 开发简单，技术栈简单
- 快速响应需求（前期阶段）；（后期随着模块增大，耦合度增大，项目臃肿，需要进行回归测试，效率下降）

分布式微服务架构







