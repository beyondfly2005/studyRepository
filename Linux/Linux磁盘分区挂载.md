# Linux磁盘分区挂载

## 硬盘管理

### MBR分区

Master Boot  Record 主引导记录

硬盘的0注明0磁头1扇区

第三部分Magic number 占2个字节 固定位55AA



添加管理步骤

添加硬盘 虚拟机 关机 添加硬盘

```bash

fdisk

#查看可用存储设备
fdisk -l 

#比如有两个硬盘
# /dev/sda
# /dev/sdb

# 选择/dev/sda进行分区
##fidisk /dev/sda
fidisk /dev/nvme0n1

# 输入入m 查看帮助
    m 查看帮助
    a 设置启动分区
    d 删除分区
    m 打印帮助页面
    n 添加一个新分区
    p 打印分区表
    q 退出 不保存
    w 报存并退出
# 输入n 创建新的分区 
   p primary 主分区
   e extended 扩展分区
# 分区编号 Partition number

# 开始位置 First sector 按默认即可
# 结束位置 Laster sector

# p 分区表
n
e
w 保存

# 重新获取分区表


partprobe /dev/sda

RHEL6操作系统

# 格式化分区
RHEL5
RHEL6
RHEL7

# 创建挂载点
创建目录 mkdir /

#查看挂载状况
df -h

#
mount 
mount | grep sda5

#开机自动挂载
fstable
ls /etc/fstable

vim /etc/fstable
#写入
/dev/sda5 /sda5 xfs defaults 0 0 
要挂载的分区 挂载点 文件系统乐行 挂载选项 是否备份 是否监测

用uuid方式写挂载文件
用uuid实现开机自动挂载

通过 blkid 查看UUID
blikid | grep sda5
vim /etc/fstable
UUID=xxx /sda5 xfs defaults 0 0 

umont /dev/sda5
mont -a
df -h

# 光盘的开机自动挂载
/dev/sr0 





```


### LVM分区创建

```bash
fdisk -l  查看磁盘列表

找到新的磁盘名为 /dev/bdb

fdisk /dev/vdb 对这块新的磁盘进行分区

n 创建新分区

p 创建主分区

t 更改分区类型

8e 修改为LVM类型分区

w 保存退出



#依次创建PV VG LV

pvcreate /dev/vdb1

vgcreate vg1 /dev/vdb1

lvcreate -L 499G  -n lvm1 vg1

# 格式化为ext4格式
mkfs .ext3 /dev/vgi/lvm1

# 挂载到/home目录
mount /dev/vg1/lvm1 /home

# 挂载命令写入fstab文件作为永久写入
echo /dev/vg1/lvm1 /home ext4 defaults 0 0 >> /etc/fstab

# 查看挂载是否成功
df -h

#用lvresize命令扩容100G
lvresize -L +100G /dev/vg1/lvm1

#使用resize2fs命令刷新新磁盘
resize2fs /dev/vg1/lvm1

# 检查磁盘容量
df -h
```



### LVM逻辑卷

> 视频资源
>
> https://www.bilibili.com/video/BV16W411t7YY?from=search&seid=12251030730903269571
>
> 

##### 传统磁盘管理存在的问题

- 无法扩展容量 只能添加硬盘 创建新的分区来扩充空间
- 没有办法动态管理磁盘空间

##### LVM逻辑卷

​	LVM(Logical volumn Manager)逻辑卷管理通过将底层物理硬盘抽象封装起来，以逻辑卷的形式表现表现给上传系统，逻辑卷的大小可以动态调整，而且不会丢失现有数据， 新家如的硬盘也不会改变现有上传的逻辑卷。

作为一种动态磁盘管理机制，逻辑卷技术达到提供了磁盘管理的灵活性

windows Server2008 新版称为 动态卷技术也与逻辑卷原理类似



##### LVM概念

- PE （physical Extend） 物理拓展
- PV（physical volume）物理卷

- VG（volume Group） 卷组

- LV（logical volume）逻辑卷



将物理硬盘格式化为物理卷PV，空间被分为一个个PE，相当于创建多个 PE  每个PE默认4M大小

不同的PV加入同一个VG，不同PV的PE全部加入VG的PE池内

LV基于PE创建 大小为PE的整数倍，组成LV的PE可以来找不同的物理磁盘

LV现在就可以格式化后直接使用了

LV的扩充缩减实际就是增加或减少促成LV的PE的数量，其过程不丢失原始数据

创建VG卷组 用来装PE 将多个PV加入到VG中

创建逻辑卷LV 

创建好一个卷组 就会多出一个以卷组名字 命名的

/dev/vgname/lvname

LV 不够用 从VG池中获取添加

VG不够用 添加硬盘 假的VG池



##### 创建LVM 

1、将物理磁盘设备初始化为物理卷

```bash

$ pvcreate /dev/sdb /dev/sdc  # sdb sdc 为新添的两块磁盘

#可以使用pvdusplay和pvs命令查看

$ pvdisplay 详细

$ pvs

# 如果只新添加了一块硬盘

$ pvcreate /dev/sdb
```

2、创建卷组 并将pv加入卷组中
```bash

$ vgcreate  linuxcost  /dev/sdb  /dev/sdc

# linuxcost 是卷组名称 sdb sdc为需要添加的的两块新磁盘

# 可以使用vgs和vgdisplay命令查看
$ vgs

$ vgdisplay        
```

3、基于卷组创建逻辑卷
```bash
$ lvcreate -n mylv -L 2G  linuxcost

# -n指定名称 -L指定大小  linuxcost从哪个卷组拿空间

$ lvdisplay

$ lvs 
```
4 位创建好的逻辑卷创建文件系统

```bash
$ mkfs.ext4 /dev/linuxcost/mylv
$
```

5 将格式化好的逻辑卷挂载使用

```bash
$ mount /dev/linuxcost/mylv /mnt
$
```

##### 删除LVM

```bash
# 删除lv
lvremove /dev/linuxcost/mylv

# 删除vg
vgremore linuxcost

# 删除物理卷
pvremove /dev/sdb
```



##### LVM的扩容（拉伸）

```bash
# 逻辑卷的拉伸操作时可以在线执行，不需要协助逻辑卷

# 1、保证vg中中有足够的空闲空间
$ vgdisplay
$ vgs

# 2、扩充逻辑卷
$ lvextend -L +1G /linuxcost/mylv

# 3、 查看扩容后的lv大小
$ lvdisplay

# 4、更新文件系统
$ resize2fs /dev/linuxcost/mylv

# 5、查看更新后文件系统
$ df-h

```

##### 拉伸逻辑卷

```bash
# 保证卷组vg中有足够的空闲空闲
vgs
```

#### 缩小逻辑卷

```bash
# 逻辑卷的缩小无法在线执行 必须离线执行

# 严格按以下步骤执行
# 1、卸载已经挂载的逻辑卷
 umount /dev/linuxcost/mylv
 
#2、缩小文件系统 
resize2fs /dev

#3、缩小lv
lvreduce -L -1G /dev/linuxcost

#4 查看缩小
lvdisplay

#5、挂载
mount /dev/
```



rp -qa | grep lvm
