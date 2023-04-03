# Linux LVM扩容

#### 虚拟机实验过程

```bash
# 修改ip
setup

# 重启网络服务
service network restart

# fdisk 分区过程
$ fdisk -l

#显示有 /dev/sdb 设备
$ fdisk /dev/sdb

m 帮助
p 打印分区表
n 新建分区
w 保存退出
q 不保存退出
# No free sectors available

#输入m 查看帮助
m

# 1- 输入n 创建一个分区
n

# 2- Partition type 选择分区类型:
#   p   primary (2 primary, 0 extended, 2 free)
#   e   extended
# 输入p 创建主分区
p

# 3- 输入分区号 按默认即可

# 4- 设置分区大小 默认将剩余全部分配给新分区

# 5- 输入t 改变主分区类型  change a partition's system id
t

# 6- 输入8e 改为分区修改为linux LVM 分区类型
8e

# 7- 输入w保存退出

#强制重读分区表
$ partprobe 

# sdb1创建为物理卷
$ pvcreate /dev/sdb1

# 查看pv
$ pvs
$ pvscan 
$ pvdisplay

# 加入到卷组
# 创建卷组
$ #vgcreate 卷组名 物理卷
$ vgcreate vg1     /dev/sdb1 /dev/sdc  将两个物理卷 /dev/sdb1 /dev/sdc 创建为卷组

# 查看vg
$ vgs
$ vgscan
$ vgdisplay


# 创建lV
$ lvcreate -L 2G -n lv1 vg0
# 命令解析：
# -L  指定大小
# -n  指定名称
# vg0 从哪个卷组创建
$ lvcreate --name centos7new -l 100%FREE vg1
# 将vg1卷组剩余的100%空间都创建为lv 并且命名为centos7new

# 将磁盘/dev/vdc 加入到卷组；前提是：/dev/vdc 已经创建为pv
$ vgextend VolGroup01 /dev/vdc 

# 查询卷组
$ vgs
#-- 默认vg名为centos

$ vgextend centos /dev/sdb1  
#-- 将新的分区sdb1 加入到名为centos的vg卷组中

# 再次查看vgs
$ vgs 
# 有了2G的空闲空间

# lv拉伸之前保证 vg中有足够的空闲空间
$ vgdisplay

#将逻辑卷名称为/dev/centos/var的逻辑卷 增加1G空间
# 增加 1G
$ lvextend -L +1G /dev/centos/var

```

#### 物理机操作记录
##### 1、将剩余空间 进行分区

```bash
#查看磁盘分区
ls /dev/
#
fdisk -l
# 磁盘名称 /dev/nvme0n1
fdisk /dev/nvme0n1
m        # help
p        # 打印分区表
n        # 创建新分区
w        # write table to disk and exit 保存退出
q		 # quit without saving changes 退出不进行保存
#强制重读分区表
partprobe 
```



##### 2、将新分区 创建PV 物理卷

```bash
fdisk -l # 不显示分区名称  新创建的分区4 无法确定分名称 所有也无法创建pv
# 通过pvscan 命令扫描 看到如下字样
/dev/nvme0n1p1           952M   12M  941M   2% /boot/efi
/dev/nvme0n1p2           1.9G  117M  1.7G   7% /boot
/dev/mapper/centos-root  197G  5.1G  182G   3% /
/dev/mapper/centos-var    94G   94G   20K 100% /var


#通过 fdisk -l
 1         2048      1953791    953M  EFI System      EFI System Partition
 2      1953792      6049791      2G  Microsoft basic 
 3      6049792    683298815    323G  Linux LVM       
 4    683298816   1000215182  151.1G  Linux filesyste 
 
# 磁盘名为 /dev/nvme0n1
#对比可知 
# 第一个分区名为 /dev/nvme0n1p1
# 第二个分区名为 /dev/nvme0n1p2
# 第三个分区被创建为了lvm 并创建的多个逻辑盘
/dev/mapper/centos-root
/dev/mapper/centos-root
# 推理可知 新创建的第4个分区 名应为 /dev/nvme0n1p4

# 对新创建的第四个分区 进行pv创建
pvcreate /dev/nvme0n1p4

# 查看创建的pv
$ pvs
$ pvscan
$ pvdisplay

# 这三个命令都可以查看到新创建的pv
 /dev/nvme0n1p3 centos lvm2 a--   322.93g       0 
 /dev/nvme0n1p4        lvm2 ---  <151.12g <151.12g

```



##### 3、将物理卷加入 卷组进行卷组扩容

```bash
$ vgs
$ vgscan
$ vgdisplay

#可以看到如下信息 ，即 之前创建的vg卷组名为 centos 需要多这个卷组进行扩容
#  VG     #PV #LV #SN Attr   VSize   VFree
#  centos   1   3   0 wz--n- 322.93g    0 

# 将新的创建的分区4 名为/dev/nvme0n1p4 加入到名为centos的vg卷组中
$ vgextend centos /dev/nvme0n1p4

# 提示扩展成功
 Volume group "centos" successfully extended

# 再次通过 vgs cgscan vgdisplay 进行查看
$ vgs
$ vgscan
$ vgdisplay

# 显示还有150多个G 空闲，刚好是新创的的第4个分区的容量
#  VG     #PV #LV #SN Attr   VSize    VFree  
#  centos   2   3   0 wz--n- <474.05g 151.11g

```



##### 4、将容量不足的逻辑卷var 进行拉伸扩容

```bash
# var逻辑卷 容量不足
$ lvs
  root centos -wi-ao---- <200.00g                                                    
  swap centos -wi-ao----   29.80g                                                    
  var  centos -wi-ao----   93.13g    
# 可以看到
$ df -h
# 结果为
Filesystem               Size  Used Avail Use% Mounted on
devtmpfs                  16G     0   16G   0% /dev
tmpfs                     16G     0   16G   0% /dev/shm
tmpfs                     16G  1.6G   15G  10% /run
tmpfs                     16G     0   16G   0% /sys/fs/cgroup
/dev/mapper/centos-root  197G  5.1G  182G   3% /
/dev/nvme0n1p2           1.9G  117M  1.7G   7% /boot
/dev/nvme0n1p1           952M   12M  941M   2% /boot/efi
/dev/mapper/centos-var    94G   94G   20K 100% /var

# 可以看到 名为 /dev/mapper/centos-var 的逻辑卷已经 空间不足 占用 100% 只有20k的容量
# 需要对其进行拉伸 扩展容量
#lvextend -L +1G /dev/mapper/centos-var
$ lvextend -L +1G /dev/mapper/centos-var

# 提示如下
  Size of logical volume centos/var changed from 93.13 GiB (23842 extents) to 94.13 GiB (24098 extents).
  Logical volume centos/var successfully resized.

# 查看拉伸后的逻辑卷大小
$ lvs

# 再次查看空间
$ df -h
# 空间并没有变化 需要更新文件系统
$ resize2fs /dev/mapper/centos-var

# 报错
resize2fs: Bad magic number in super-block while trying to open /dev/mapper/centos-var
Couldn't find valid filesystem superblock.

#需要修改命令使用xfs_growfs
$ xfs_growfs /dev/mapper/centos-var


# 提示成功
meta-data=/dev/mapper/centos-var isize=512    agcount=4, agsize=6103552 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=24414208, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=11921, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 24414208 to 25200640


# 再次使用 df -h 查看文件占用
$ df -h
/dev/mapper/centos-var    97G   94G  3.0G  97% /var

```

