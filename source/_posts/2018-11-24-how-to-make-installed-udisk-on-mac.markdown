---
layout: post
title: "Mac上面制作Ubuntu启动U盘"
date: 2018-11-24 10:50:23 +0800
comments: true
categories: mac
---

> Mac下面制作Ubuntu启动U盘
* 下载Ubuntu.iso镜像文件(以下命令执行须跟镜像文件同目录)
* 查看USB盘符
* diskutil list
* 卸载USB磁盘
* diskutil unmountDisk /dev/disk2（找到对应的设备）
* 使用dd将ubuntu的（ISO）img写入U盘
* sudo dd if=ubuntu-desktop-i386.img.dmg of=/dev/disk2 bs=1m
* 或者 sudo dd if=ubuntu-desktop-i386.iso of=/dev/disk2 bs=1m
* 弹出USB(现在不用这一步，上一步写入完毕之后，会自动弹框说U盘不可读，点击退出即可)
* diskutil eject /dev/disk2

> Mac 下面格式化制作的启动盘
```
    
    diskutil list
    diskutil eraseVolume JHFS+ UntitledHFS /dev/disk2
    上一条不对的话，根据提示操作
    sudo diskutil umountDisk /dev/disk2
    diskutil list
    列出资盘，并查看/dev/disk2的大小
```

> MacOS 磁盘管理工具 diskutil 介绍
- [MacOS 磁盘管理工具 diskutil 介绍](https://www.jianshu.com/p/6a1f365617ad)