

### Windows下查看端口占用



1、按 win+R，点击运行页面，写入cmd回车，点击命令行页面



2、使用命令查看端口，这里查看10000端口；

```bash
netstat -aon|findstr "10000"
netstat -aon|findstr "11100"
```



4、然后，使用tasklist命令查看进程；

```bash
tasklist|findstr "9904"
```

