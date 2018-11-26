---
layout: post
title: "vagrant 基础"
date: 2018-11-23 14:41:29 +0800
comments: true
categories: vagrant
---

## VirtualBox

**安装 VirtualBox**

    wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- \
                    | sudo apt-key add -
    echo  "deb https://mirrors.tuna.tsinghua.edu.cn/virtualbox/apt/ xenial contrib" \
                    | sudo tee -a /etc/apt/sources.list.d/virtualbox.list
    sudo apt-get update
    sudo apt-get install virtualbox-5.0

**安装 VirtualBox Extension Pack(可选)**

* [VirtualBox Extension Pack](http://download.virtualbox.org/virtualbox/5.0.26/Oracle_VM_VirtualBox_Extension_Pack-5.0.26-108824.vbox-extpack)


##  vagrant
**vagrant 相关文章及资源**

* [使用Vagrant打造跨平台开发环境](https://segmentfault.com/a/1190000000264347)
* [开启 NFS 文件系统提升 Vagrant 共享目录的性能](https://segmentfault.com/a/1190000000270453)

**安装**

    sudo apt-get install vagrant

**Box使用**

* 下载 [trusty-server-cloudimg-amd64-vagrant-disk1.box](
    https://mirrors.tuna.tsinghua.edu.cn/ubuntu-cloud-images/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box)(BIOS需要开启虚拟化)
    
* 下载 [trusty-server-cloudimg-i386-vagrant-disk1.box](https://mirrors.tuna.tsinghua.edu.cn/ubuntu-cloud-images/vagrant/trusty/current/trusty-server-cloudimg-i386-vagrant-disk1.box)


    vagrant box add base trusty-server-cloudimg-amd64-vagrant-disk1.box
    

* `vagrant box list`: 列出已加载的Box
* `vagrant box add` : 加载新的Box
* `vagrant box remove`: 移除加载的box
* `vagrant init` : 初始化虚拟机，并产生Vagrantfile文件
* `vagrant up` :启动虚拟机
* `vagrant ssh`: 进入虚拟机
* `vagrant halt` : 关闭虚拟机
* `vagrant reload` : 重启虚拟机
* `vagrant status` : 查看虚拟机的运行状态
* `vagrant destroy` : 销毁虚拟机
* `vagrant package` :打包虚拟机