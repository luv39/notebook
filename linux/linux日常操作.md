# Linux日常操作

[TOC]

## linux设置开机自启动脚本

* 编辑rc.local脚本

Ubuntu开机之后会执行/etc/rc.local文件中的脚本，所以我们可以直接在/etc/rc.local中添加启动脚本。添加到语句'exit 0'前面。

* 添加一个Ubuntu的开机启动服务

如果要添加为开机启动执行的脚本文件，可先将脚本复制或软链接到/etc/init.d/目录下，然后用'update-rc.d xxx defaults NN'(NN为启动顺序)命令将脚本添加到初始化执行的队列中去。

```sh
cd /etc/init.d
sudo update-rc.d test default 95
```

卸载启动脚本的方法：

```sh
cd /etc/init.d
sudo update-rc.d -f test remove
```

* 每次登陆自动执行

在`/etc/profile.d/`目录下新建sh脚本，`/etc/profile`会遍历`/etc/profile.d/*.sh`

## linux下硬盘分区和格式化

* fdisk 命令用于硬盘分区

1. 使用`fdisk -l`查看硬盘的详细信息
2. 分区初始化`fdisk /dev/sda`

> 各个参数的解析：
> 输入m显示所有命令
> 输入p显示硬盘分割情形，打印分区表
> 输入a设定硬盘启动区
> 输入n设定新的硬盘分割区
> > 输入p硬盘为主要分割区（primary）
> > 输入e硬盘为扩展分区（extend）

> 输入t改变硬盘分割区属性
> 输入d删除硬盘分割区属性
> 输入q结束不存入硬盘分割区属性
> 输入w结束并写入硬盘分割区属性

* mkfs 命令用于建立文件系统，格式化

```sh
mkfs -t ntfs /dev/sda1
```

## linux下挂载硬盘分区

* 更改`/etc/fstab`永久性挂载配置

```sh
<file system>         <mount point>  <type>   <options>  <dump>  <pass>
PARTUUID=01392383-01  /boot           vfat    defaults     0       2
```

|    名称     |                             解释                             |
| :---------: | :----------------------------------------------------------: |
| file system | 描述要挂载的特殊的块设备或远程文件系统，如/dev/cdrom /dev/sdb等，远程文件系统使用host:dir |
| mount point | 描述文件系统的挂载点；如果是一个交换分区(swap partitions)，这个域应写为‘none’ |
|    type     | 描述文件系统的类型，Linux支持许多文件系统类型，如adfs, affs, autofs, coda, coherent, cramfs,devpts, efs, ext2, ext3, hfs, hpfs, iso9660, jfs, minix, msdos, ncpfs, nfs,ntfs, proc, qnx4, reiserfs, romfs, smbfs, sysv, tmpfs, udf, ufs, umsdos,vfat, xenix, xfs,等 |
|   options   |                描述关于这个文件系统的挂载选项                |
|    dump     | 当其值设置为1时将允许dump备份程序备份；设置为0时忽略备份操作；如果文件系统需不需要被dump，则设置为0即可 |
|    pass     | 该字段由fsck程序用于确定在重新启动时文件系统检查完成的顺序，启动用的文件系统需要制定为1，其他文件系统需要指定为2，如果没有此域或设置为0表示不检查。其值是一个顺序。当其值为0时，永远不检查；而 / 根目录分区永远都为1。其它分区从2开始，数字越小越先检查，如果两个分区的数字相同，则同时检查 |

* mount 临时挂载

> mount：查看分区格式
> > -a 挂载信息立即生效
> > -t ext3 /dev/sdb1 /opt 临时挂载linux分区
> > -t vfat /dev/sdc1 //media/usb u盘挂载 window分区
> > -o loop docs.iso /media/iso 挂载镜像文件

> mount media/cdrom 光驱挂载
> umount /opt 卸载挂载

* `df -h`: 查看当前硬盘使用情况

## samba服务器配置

## linux服务管理

* systemd

许多Linux的distributions都已经转投systemd了，而Ubuntu自从15.04以后都使用了systemd
> 常用的命令:
> > 打开服务：sudo systemctl start foobar
> > 关闭服务：sudo systemctl stop foobar
> > 重启服务：sudo systemctl restart foobar
> > 不中断正常功能下重新加载服务：sudo systemctl reload foobar
> > 设置服务的开机自启动：sudo systemctl enable foobar
> > 关闭服务的开机自启动：sudo systemctl disable foobar
> > 查看活跃的单元：systemctl list-units
> > 查看某个服务的状态：systemctl status foobar