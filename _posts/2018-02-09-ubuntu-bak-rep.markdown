---
layout:     post
title:      "Ubuntu全盘备份与恢复"
subtitle:   "Full backup and recovery of Ubuntu "
date:       2018-02-09 12:00:00
author:     "Eric"
header-img: "img/ubuntu-1234.jpg"
header-mask: 0.3
catalog:    true
tags:
    - ubuntu
    - Linux
---




## Ubuntu全盘备份与恢复

初学者在使用Ubuntu这类Linux操作系统时，常常会由于不当操作导致系统崩溃，重装系统是难免的事情。重装系统虽然简单，但是各种软件/环境重新下载、配置相当占用时间，因此有必要对系统同进行备份，以避免将来连哭的地方都找不到。本教程已经由JK小僧在计算机上亲自实践，可以恢复之前Ubuntu系统上的各类软件和环境配置，方法简单，值得推广：

### 一、备份系统
> 使用TAR。和Windows不同，Linux不会限制root访问任何东西，你可以把分区上的所有东西都扔到一个TAR文件里去

  - 1. 打开一个终端，并运行 sudo su（回车后要求输入密码）；

  - 2. 继续在终端中输入以下指令，进入系统根目录： 

  ```bash
  $ cd /
  ```

  - 3. 开始备份系统，在终端中输入：
    
  ```bash
  $ tar cvpzf backup.tgz --exclude=/proc --exclude=/lost+found --exclude=/backup.tgz --exclude=/mnt --exclude=/sys --exclude=/media /
  ```

让我们来简单看一下这个命令：

* **tar**: 是用来备份的程序
* **c -**: 新建一个备份文档
* **p -**: 保存许可，并应用到所有文件
* **z -**: 采用‘gzip’压缩备份文件，以减小备份文件体积
* **f -**: 说明备份文件存放的路径， Ubuntu.tgz 是本例子中备份文件名
* **r'/'**: 是我们要备份的目录，在这里是整个文件系统
* 在档案文件名 **“backup.tgz”** 和要备份的目录名 **“/”** 之间给出了备份时必须排除在外的目录。有些目录是无用的，例如 **“/proc”、“/lost+ found”、“/sys”** 。当然， **“backup.tgz”** 这个档案文件本身必须排除在外，否则你可能会得到一些超出常理的结果。如果不把“/mnt”排除在外，那么挂载在“/mnt”上的其它分区也会被备份。另外需要确认一下 **“/media”** 上没有挂载任何东西(例如光盘、移动硬盘)，如果有挂载东西， 必须把 **“/media”** 也排除在外.
* 备份完成后，在文件系统的根目录将生成一个名为 **“backup.tgz”** 的文件，它的尺寸有可能非常大。现在你可以把它烧录到DVD上或者放到你认为安全的地方去。       
**在备份命令结束时你可能会看到这样一个提示：’tar: Error exit delayed from previous errors’，多数情况下你可以忽略它。**

### 二、恢复系统

  - 1.如果原来的Ubuntu系统已经崩溃，无法进入。则可以使用Ubuntu安装U盘（live USB）进入试用Ubuntu界面。

  - 2.切换到root用户，找到之前Ubuntu系统的根目录所在磁盘分区 **（一般电脑上的磁盘分区（假设分区名称为sdaX）均可以在当前Ubuntu系统的根目录下的media目录下（即/media）找到。目录通常为当前根目录下 cd /media/磁盘名称/分区名称）** 。进入该分区，输入以下指令来删除该根目录下的所有文件： 
  ```bash
  $ sudo rm -rf /media/磁盘名称/分区名称*
  ```
  - 3.将备份文件”backup.tgz”拷入该分区；
  ```bash 
  $ sudo cp -i backup.tgz /media/磁盘名/分区名sdaX
  ```
  - 4.进入分区并将压缩文件解压缩，参数x是告诉tar程序解压缩备份文件。 
  ```bash
  $ sudo tar xvpfz backup.tgz
  ```
  - 5.重新创建那些在备份时被排除在外的目录； 
  ```bash
  $ sudo mkdir proc lost+found mnt sys media 
  ```
  或者这样：   
  mkdir proc   
  mkdir lost+found    
  mkdir mnt    
  mkdir sys   

当你重启电脑，你会发现一切东西恢复到你创建备份时的样子了！
