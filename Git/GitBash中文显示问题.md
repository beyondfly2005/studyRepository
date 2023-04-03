# Git常见问题处理

## 解决Git终端Git Bash中中文乱码（不显示中文）问题

> https://blog.csdn.net/qq_17229141/article/details/105160211


#### 1、设置本地字符集

最好先设置一下我们的终端显示的编码（大概是这么叫吧）

右键终端上面的标题栏，选择Options->Text

设置Locale字符集为：zh_CN 字符集为UTF-8



#### 2、执行命令

直接在终端中执行下面的命令

**git config --global core.quotepath false**

之后再执查看命令，就看到我们显示的列表中中文名字正常显示了。
