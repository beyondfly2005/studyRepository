# Ansible

### 什么是Ansible

Ansible是一个开源部署工具

采用python语言开发 

playbook为基础部署架构 

特点：SSH协议通讯，全平台，无需编译 模块化部署

 仅依赖系统标配的SSH连接与授权管理 就可以进行远程部署

作用推送playbook进行远程节点快速部署



### Ansible与Chef Saltstack的不同

- Chef

  Chef 是Ruby语言编写 CS架构 配置需要依赖Git

  Recipe脚步规范编写，需要Ruby编程经验

- Saltstack
	Python 语言编写 CS架构 模块化配置管理 YAML脚本编写规范 ，适合大规模集群部署
	需要在目标主机安装客户端
	
- Ansible
 	Python语言编写，无Client 模块化配置管理；
 	Playbook脚本编写规范，易于上手，适合中小规模快速部署

### Ansible的优势和应用场景
- 轻量级无客户端(Agentless)
	SSH安全连接 减少了系统安全问题 
	不需要部署客户端工具 减少了资源(内存 硬盘 cpu)消耗
- 开源免费 学习成本低 快速上手
- 使用Playbooke 作为核心配置架构，统一的脚本格式批量化部署

- 完善的模块化扩展 支持主流的开发场景
- 强大的稳定性和兼容性 采用python 和ssh 均为系统自带
- 活跃的官方舍弃，方便问题解决/

### Ansible

- 运维自动化发展历程及技术应用
- Ansible命令使用
- Ansible常用模块详解
- Yaml语法简介
- Ansible playbook基础
- PlayBook变量 tags handlers使用
- PlayBook模板templates
- PlayBook条件判断when
- PlayBooks字典 with_items
- Ansible Roles 角色

#### 运维自动化发展历程及技术应用

- 本地部署On-premises
- 基础设施即服务IasS  Infrastructure as a Service
- 平台即服务 PasS Platform as a Service
- 软件即服务Saas Soft as a Service

#### Ansible工作架构和一个原理



