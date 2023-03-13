# Vue

>  https://www.bilibili.com/video/BV1op4y1U7Bz?p=1

#### Vue-cli4脚手架搭建

Vue-cli4.1.1版本

##### 1、安装node.js

node 版本10.0以上

##### 2、安装cnpm

```shell
# 查看cnpm是否安装
cnpm -v

# 安装cnpm
npm install -g cnpm --registry=https://registry.npm.taobao.org

```

##### 3、安装vue-cli脚手架构建工具

```bash
vue -V
cnpm install -g @vue/cli
# 安装指定版本
# 如果是3.0以下的版本
npm install -g vue-cli@版本号
# 如果是3.0以上的版本
npm install -g @vue/cli@版本号
```

4、创建项目

```bash
vue create 项目名称
```

Cmder工具

路由模式：

history模式 需要后端支持

hash模式 地址栏有#号

```
npm -i
cnpm -i
```

package.json

main.js 项目的入口文件

```
render :h=>h(App)  //h 是 createElement的别名
```

相当于ES5中如下写法

```
render:function(createElement){
	return createElement(App);
}
```

组件加载的两种方式

import 

