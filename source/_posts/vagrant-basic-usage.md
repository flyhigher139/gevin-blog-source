---
layout: post
title: vagrant使用简介
date: 2014-11-13
category: tech
tags: vagrant
description: 本文是介绍了vagrant基本使用
---


#Outline
- 安装

- 初始化工作环境
  - 初始化工作目录
  - 首次启动vagrant
  - 使用vagrant中的ubuntu环境
  - 同步的目录

- 基本用法
---
>Vagrant provides easy to configure, reproducible, and portable work environments built on top of industry-standard technology and controlled by a single consistent workflow to help maximize the productivity and flexibility of you and your team.

>-- [WHY VAGRANT?](https://docs.vagrantup.com/v2/why-vagrant/index.html)

---

#安装
使用vagrant时，会启动虚拟机，默认使用的虚拟机是virtualbox，所以使用vagrant前，首先安装`virtualbox`和`vagrant`

#初始化工作环境

##初始化工作目录

创建一个开发环境的根目录，如`E:\projects\vagrant-project1`

cmd中，cd到该根目录，使用下面命令，会初始化工作目录，并在工作目录下创建配置文件`vagrantfile`，配置文件中设置了该工作目录的开发环境为`ubuntu-14.04-amd64`

```
vagrant init phusion/ubuntu-14.04-amd64
```

##首次启动vagrant

启动vagrant的命令如下：
```
vagrant up
```
启动vagrant后，会自动开启虚拟机。由于我们初始化工作目录时，使用的开发环境为`phusion/ubuntu-14.04-amd64`，现在系统中没有这个文件，因此执行`vagrant up`时，首先会下载此文件到本地目录。`phusion/ubuntu-14.04-amd64`放在vagrant的公共目录，因此以后初始化其他工作环境时，如果使用这个文件，也不需要继续下载

##使用vagrant中的ubuntu环境

使用ubuntu环境，需要ssh登录虚拟机

由于Windows系统的命令行中，没有自带ssh客户端，因此需要借助putty。

登录相关信息如下：
```
username: vagrant
password: vagrant
port: 2222
```

除了上述方法，在集成了ssh的系统中，还可以直接通过下面命令进入虚拟机环境：
```
vagrant ssh
```

##同步的目录

默认设置下，上面的工作环境根目录`E:\projects\vagrant-project1`与虚拟机中的目录`/vagrant`是一致的，始终同步，可以在虚拟机中使用命令`ls /vagrant`查看目录文件，内容与主机开发环境根目录完成一样。主机开发环境根目录下对文件的所有读写操作，都会同步到虚拟机的`/vagrant`中

可以[修改vagrantfile文件](http://docs.vagrantup.com/v2/synced-folders/basic_usage.html)，添加更多同步目录

#基本用法

- 启动虚拟机
`vagrant up`

- 登录虚拟机
`vagrant ssh`
Windows命令行中没有安装ssh client的话，请使用putty

- 销毁虚拟机
`vagrant destory`

- 关闭虚拟机
`vagrant halt`

- 挂起虚拟机
`vagrant suspend`

- 恢复虚拟机
`vagrant resume`

