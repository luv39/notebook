# 文件
## 打包文件
* 创建一个新的归档文件 `tar -cf guidang01.tar wenjian01 wenjian02 wenjian03`
* 向归档文件末尾追加文件 `tar -f guidang01.tar -r wenjian04`
* 从归档文件中解出文件 `tar -xf guidang01.tar -C ./`
* 列出归档文件中的文件 `tar -tf guidang01.tar`
* 从归档文件中删除一个文件 `tar -f guidang01.tar --delete wenjian04`
* 合并两个归档文件 `tar -f guidang01.tar -A guidang02.tar`
* 调用gzip来压缩/解压缩文件 `tar -czf guidang.tar.gz wenjian01 wenjian02` / `tar -xzf guidang.tar.gz`

## 压缩文件
* gzip压缩文件 `gzip guidang.tar`
* gzip解压文件 `gzip -d guidang.tar.gz`
* 改变压缩比 `gzip -1 guidang.tar` (1到9,9压缩率最大)

## 查找文件
> locate
* `locate 查找路径 关键字`
* Linux将所有的文件名都记录在`/var/lib/mlocate`中,locate命令到数据库中去查找,所以比较快,但是这个数据库默认每天更新一次,所以可能查找得不准确,我们也可以手动更新数据库,用`updatedb`命令.

> find
* `find [查找范围][查找条件][动作]`
* 根据文件名字(精准名字)查找文件 `find / -name passwd`
* 根据文件属性查找文件 `find / -type l`
* 根据时间属性查找文件

| | | |
|:---:|:---:|:---:|
| -mtime | -mmin | 文件被修改的时间
| -ctime | -cmin | 文件被读取/执行的时间
| -atime | -amin | 文件属性修改时间 
| | |

* 查找3天(分钟)内修改过的文件 `find /tmp -mtime(-mmin) -3`
* 查找3天(分钟)前修改过的文件 `find /tmp -mtime(-mmin) +3`
* 查找3天前(分钟)那天修改过的文件 `find /tmp -mtime(-mmin) 3`
* 根据文件大小查找文件 `find /tmp -size -3c`

| 符号 | 含义 |
|:---:|:---:|
| c | 字节 |
| k | 1024字节 |
| M | 1024k |
| G | 1024M |
| - | 小于 |
| + | 大于 |


# 磁盘
## 挂载硬盘分区

* 显示硬盘挂载情况命令`df -l`
* 新硬盘挂载命令
```
sudo mount -t ntfs /dev/sdb1 /media/wd
指定硬盘分区文件类型为ntfs,同时将/dev/sdb1挂载到/media/wd
```
* 查看硬盘的UUID命令`blkid /etc/sdb1`
* 配置硬盘在系统启动自动挂载
```
在文件 /etc/fstab 中加入如下配置:
# /dev/sdb1 was on /media/wd
UUID=37eaa526-5d96-4237-8468-603df5216ce9     /media/wd     ntfs     defaults     0     0
```
