---
title: 手机linux服务器
tags: ['学习']
categories: 学习
date: 2022-03-23

---

旧安卓手机改造服务器。

<!--more-->

### 1. 流程

1. 手机安装 [f-droid](https://f-droid.org/)

2. f-droid 安装 temux

3. temux操作

   1. [切换源](https://mirrors.tuna.tsinghua.edu.cn/help/termux/)

      `termux-change-repo`

   2. 更新包

      `apt update`

      `pkg update`

      `apt upgrade`

   3. 安装ssh

      `apt install openssh`

      `pkg install termux-auth`

   4. 获取用户名

      `whoami  // 如：a0_a62`

   5. 设置密码

      `passwd`

   6. 查看本机局域网ip in中的是

      `ifconfig`

   7. 手机启动ssh服务 

      `sshd`

### 2. 坑

1. git clone 报错 library "libssl.so.1.1" not found

   解决链接：[csdn](https://blog.csdn.net/qq_42560204/article/details/125670804)

   解决方法：

   1. 安装openssl1.1
      先搜索 `pkg search openssl1.1`
      可以看到有一个openssl1.1-tool的package，对它进行安装`pkg install openssl1.1-tool` 

      已经安装openssh跳过这步

   2.  搜索libssl.so.1.1
      可以pwd先看下自己的目录 一般来说都安装到了/data/data/com.termux/files下

      `find /data/data/com.termux/files -name 'libssl.so.*'`

   3. 添加环境变量
      `echo "export LD_LIBRARY_PATH=/data/data/com.termux/files/usr/lib/openssl-1.1" >> ~/.bashrc`
      使当前shell生效

      `export LD_LIBRARY_PATH=/data/data/com.termux/files/usr/lib/openssl-1.1`

   4. 再次检验命令 ————成功

2. 修改linux文件

   `vim $PREFIX/etc/resolv.conf`

### 3. Termux启用ftp功能

openssh自带sftp-server。

连接方法:Filezilla ---> 新建链接 ---> 选择sftp协议 ---> 询问用户名和密码 (whoami和对应的密码)

注意端口选择8022，即可连接sftp服务，亲测可删写服务器内容，简单极致，无需安装和启动pure-ftpd软件。