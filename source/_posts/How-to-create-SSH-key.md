---
layout: post
title: 如何生成SSH key
date: 2014-08-28 16:42:01
category: github
tags: tip
description: 本文主要内容来源于GitHub中添加SSH Key的帮助文档
---


SSH key提供了一种与GitHub通信的方式，通过这种方式，能够在不输入密码的情况下，将GitHub作为自己的remote端服务器，进行版本控制

##步骤

- 检查SSH keys是否存在
- 生成新的ssh key
- 将ssh key添加到GitHub中

![如何生成SSH KEY](http://gevin-zone.qiniudn.com/gevin-essay-how-to-generate-SSH-key.png "如何生成SSH KEY")

##1. 检查SSH keys是否存在

输入下面的命令，如果有文件`id_rsa.pub` 或 `id_dsa.pub`，则直接进入步骤3将SSH key添加到GitHub中，否则进入第二步生成SSH key

```shell
ls -al ~/.ssh
# Lists the files in your .ssh directory, if they exist
```

##2. 生成新的ssh key

**第一步：生成public/private rsa key pair**
在命令行中输入`ssh-keygen -t rsa -C "your_email@example.com"`

默认会在相应路径下（/your_home_path）生成`id_rsa`和`id_rsa.pub`两个文件，如下面代码所示

```
ssh-keygen -t rsa -C "your_email@example.com"
# Creates a new ssh key using the provided email
Generating public/private rsa key pair.
Enter file in which to save the key (/your_home_path/.ssh/id_rsa):
```

**第二步：输入passphrase（本步骤可以跳过）**

设置passphrase后，进行版本控制时，每次与GitHub通信都会要求输入passphrase，以避免某些“失误”

```shell
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```

sample result:

```
Your identification has been saved in /your_home_path/.ssh/id_rsa.
Your public key has been saved in /your_home_path/.ssh/id_rsa.pub.
The key fingerprint is:
#01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
```
**第三步：将新生成的key添加到ssh-agent中:**

```
# start the ssh-agent in the background
eval "$(ssh-agent -s)"
Agent pid 59566
ssh-add ~/.ssh/id_rsa
```

##3. 将ssh key添加到GitHub中

用自己喜欢的文本编辑器打开`id_rsa.pub`文件，里面的信息即为SSH key，将这些信息复制到GitHub的`Add SSH key`页面即可

不同的操作系统，均有一些命令，直接将SSH key从文件拷贝到粘贴板中，如下：

**mac**
```
pbcopy < ~/.ssh/id_rsa.pub
# Copies the contents of the id_rsa.pub file to your clipboard
```

**windows**
```
clip < ~/.ssh/id_rsa.pub
# Copies the contents of the id_rsa.pub file to your clipboard
```

**linux**

```
sudo apt-get install xclip
# Downloads and installs xclip. If you don't have `apt-get`, you might need to use another installer (like `yum`)

xclip -sel clip < ~/.ssh/id_rsa.pub
# Copies the contents of the id_rsa.pub file to your clipboard
```
