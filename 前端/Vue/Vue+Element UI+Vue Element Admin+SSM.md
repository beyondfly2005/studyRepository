> https://www.bilibili.com/video/BV1YE411A746?p=1

### 一、什么是Vue

#### 1、简介

Vue是一套用于构建用户界面的**渐进式的js框架**，发布于2014年2月，与其他大象框架不同的是，Vue被设计为可以字底向上逐层应用。**Vue只关注视图层**，不仅易于上手，还便于与第三方库（如：vue-router vue-resource，vuex）或项目整合，

结合html和css+Js，易用，并且有着良好的生态

- 易用
- 高效：体积很小 虚拟DOM 最省心的优化
- 

#### 2、MVVM模式的实现者 数据双向绑定

- Model 模型车
- View 视图层
- ViewModel 连接视图和数据的中间件 Vue.js就是MVVM中ViewModel层的实现者



cdn内容分发网络

​	是一种加速策略，能够从离自己最近的服务器上快速的获取外部资源

页面中使用vue框架的两种方式：

- 引入工程内的vue.js文件
- 引入外部网络提供的vue的js文件



### 二、Vue组件

##### 事件修饰符



##### 按键修饰符





#### P40 在Idea中使用vue插件

##### 安装

settings plugins 插件是从搜索vue并安装

##### 配置

Settings -> Editor -> File Types 中找到Vue.js Template 添加*.vue使用



##### Idea 搜索不到插件处理方法

Idea -> Settings ->Appearance & Behavior -> System Settings -> Http Proxy

Auto-detect proxy settings 取消下面 Automatic proxy configuration取消 复选框的的选择

![img](https://img-blog.csdnimg.cn/20200220160823145.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyODQwOTEy,size_16,color_FFFFFF,t_70)





### P68 Vue-element-admin介绍

- Vue-admin-template的安装及构建

```bash
# 克隆项目
git clone https://github.com/PanJiaChen/vue-admin-template.git

# 进入项目目录
cd vue-admin-template

# 安装依赖
npm install

# 建议不要直接使用 cnpm 安装以来，会有各种诡异的 bug。可以通过如下操作解决 npm 下载速度慢的问题
npm install --registry=https://registry.npm.taobao.org

# 启动服务
npm run dev

#浏览器访问 
http://localhost:9528
```

