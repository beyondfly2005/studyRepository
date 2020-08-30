> 视频 https://www.bilibili.com/video/BV12J411m7MG?p=1

## 0、环境搭建


### VS Code设置中文插件

 Vscode是一款开源的跨平台编辑器。默认情况下，vscode使用的语言为英文(en)

1）打开vscode工具；

2）使用快捷键组合【Ctrl+Shift+p】，在搜索框中输入“configure display language”，点击确定后；

3）修改locale.json文件下的属性“locale”为“zh-CN”，如下图;

4）重启vscode工具；



### Vs Code常用Vue插件

- EsLint  2.1.8
- LiveServer  5.6.1
- open in browser 2.0.0
- Vetur 0.26.1
- Prettier-Code formatter 5.5.5
  - "editor.formatOnSave": true

- Preview on Web Server 1.3.0 内置浏览器

- Debugger for chrome

### Preview on Web Server 使用

#####  内置浏览器使用

```bash
快捷键  ctl + Shift + V
```



### Prettier 配置

VSCode左下角的设置图标--》设置--》输入框中搜索设置-》用户--》功能--》终端--》在settings.json中编辑--》添加如下代码

```json
/*  prettier的配置 */
    "prettier.printWidth": 100, // 超过最大值换行
    "prettier.tabWidth": 4, // 缩进字节数
    "prettier.useTabs": false, // 缩进不使用tab，使用空格
    "prettier.semi": true, // 句尾添加分号
    "prettier.singleQuote": true, // 使用单引号代替双引号
    "prettier.proseWrap": "preserve", // 默认值。因为使用了一些折行敏感型的渲染器（如GitHub comment）而按照markdown文本样式进行折行
    "prettier.arrowParens": "avoid", //  (x) => {} 箭头函数参数只有一个时是否要有小括号。avoid：省略括号
    "prettier.bracketSpacing": true, // 在对象，数组括号与文字之间加空格 "{ foo: bar }"
    "prettier.disableLanguages": ["vue"], // 不格式化vue文件，vue文件的格式化单独设置
    "prettier.endOfLine": "auto", // 结尾是 \n \r \n\r auto
    "prettier.eslintIntegration": false, //不让prettier使用eslint的代码格式进行校验
    "prettier.htmlWhitespaceSensitivity": "ignore",
    "prettier.ignorePath": ".prettierignore", // 不使用prettier格式化的文件填写在项目的.prettierignore文件中
    "prettier.jsxBracketSameLine": false, // 在jsx中把'>' 是否单独放一行
    "prettier.jsxSingleQuote": false, // 在jsx中使用单引号代替双引号
    "prettier.parser": "babylon", // 格式化的解析器，默认是babylon
    "prettier.requireConfig": false, // Require a 'prettierconfig' to format prettier
    "prettier.stylelintIntegration": false, //不让prettier使用stylelint的代码格式进行校验
    "prettier.trailingComma": "es5", // 在对象或数组最后一个元素后面是否加逗号（在ES5中加尾逗号）
    "prettier.tslintIntegration": false,
    "terminal.integrated.allowMnemonics": true,
    "terminal.integrated.automationShell.linux": "" // 不让prettier使用tslint的代码格式进行校验
///报错的话，检查一下有没有用逗号与上一项设置分隔
```



### Prettier使用

```
使用alt+shift+f组合键来快速格式化你在VS Code中的代码
```



## 1、课程介绍

##### 1、学期须知

- html
- css 
- JavaScript  
  - BOM  DOM
- AJAX

##### 1.2 开发工具

- vscode

- LiveServer

##### 1.3课程安排

- Vue基础

- 本地应用

- 网络应用

- 综合应用

## 2、Vue基础—简介

###### 1、JavaScript框架

​	JQuery是类库 

###### 2、简化Dom操作

​	不需要人为操作Dom元素

###### 3、响应式数据驱动



## 3、Vue基础—第一个Vue程序

Vue简介

第一个Vue

官方文档：http://cn.vuejs.org

创建html文件 导入vue.js文件

开发环境版本 完整版本

生成环境 min版本的jquery

声明式渲染

步骤：

- 导入开发版本的Vue.js
- 创建Vue实例对象，设置el属性和data属性
- 使用简介的模板语法把数据渲染到页面上



## 4、Vue基础—el:挂载点

- Vue实例的作用范围是什么？

  ​	Vue会管理el选型命中的元素及其元素内部的后代元素

  ​	作用范围：el命中的内部

- 是否发可以使用其他的选择器？

  可以使用其他的选择器，但是建议使用ID选择器

- 是否可以使用其他的dom元素呢？

  可以使用其他的双标签的dom元素，但是不能使用html和body

#### el: 挂载点的作用

- el是用来设置Vue实例挂载（管理）的元素

- Vue会话管理el选项命中的元素及其内部的后代元素
- 可以使用其他的选择器，但是建议使用ID选择器
- 可以使用其他的双标签，但是不能使用html和body



## 5、data:数据对象

基本类型 字符串 message

复杂类型：对象 数组

- Vue中用到的内容订阅在data中
- data中可以写复杂类型的数据
- 渲染复杂类型的数据时，遵守js的语法即可



## 6、本地应用—介绍

##### Vue指令

- v-test

- v-html

- v-on基础

- v-show

- v-if

- v-bind

- v-for

- v-on 补充

- v-model

##### 按知识点，分三部分内容

- 内容绑定，事件绑定

- 显示切换，属性绑定

- 类别循环，表单元素绑定



## 7、本地应用—v-text指令

这个标签的作用是把数据设置给标签的文本值 textContexnt的值

差值表达式 {{}}  替换部分内容

v-text 会替换所有内容

- v-text指令的作用是 设置标签的内容textContent
- 默认写法（v-text的写法）会替换全部内容，使用差值表达式{{}}可以替换指定内容
- 内部支持写表达式； 如：字符串拼接



## 8、本地应用-v-html指令

设置标签的innerHTML

- v-html指令的作用是设置元素的innerHTML

- 内容中有html结构挥别解析为html标签
- v-text指令无论内容是什么，只会解析为文本 原样输出
- 解析文本使用v-text，需要谢谢为html结构使用v-html



## 9、本地应用v-on指令基础

为元素绑定事件

onclick onmouseenter ondblclick

- v-on指令的作用是 为元素绑定事件
- 事件名不需要写on
- 指令可以简写为@
- 绑定的方法定义在methods属性中
- 方法内部通过this关键字可以访问定义在data中的数据

## 10、本地应用-计数器



##### 本章要点

- 创建vue实例时 el是挂载点 data数据 methods是方法
- v-on指令的作用是绑定事件，简写为@
- 方法中通过this，关键字获取data中的数据
- v-text指令的作用是 设置元素的文本值，简写为{{}}