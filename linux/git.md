# git



[TOC]

## git服务器搭建

这里以树莓派为例

1. 安装git

   ```shell
   $ sudo apt install git
   ```

2. 创建证书登陆

   收集所有需要登录的用户的公钥，公钥位于id_rsa.pub文件中，把我们的公钥导入到/home/pi/.ssh/authorized_keys文件里，一行一个。

   ```shell
   $ cd /home/pi/
   $ mkdir .ssh
   $ chmod 700 .ssh
   $ touch .ssh/authorized_keys
   $ chmod 600 .ssh/authorized_keys
   ```

3. 初始化Git仓库

   ```shell
   $ cd /
   $ mkdir git
   $ cd git
   
   $ git init --bare test
   Initialized empty Git repository in /git/test/
   ```

4. 克隆仓库

   ```shell
   $ git clone git@192.168.191.20:/git/test
   Cloning into 'test'...
   warning: You appear to have cloned an empty repository.
   Checking connectivity... done.
   ```

