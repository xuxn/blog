---
categories: learn
layout: post-detail
title: "安装git"
date: 2017-3-30 17:00:00 +0800 
---
#安装git
##windows安装
**********
**1. 下载git客户端**
下载地址为：<https://git-scm.com/download/mac>
**2. 打开安装包**
![git install](/blog/img/2017-3-30/1.png)
**3. git config 配置**
```
git config --global user.name shining
git config --global user.email shining@achang.com 
git config --list
```
![git install](/blog/img/2017-3-30/2.png)
```
ssh-keygen -t rsa -C "shining@achang.com"
```
![git install](/blog/img/2017-3-30/3.png)
**5. id_rsa.pub查看ssh key**
![git install](/blog/img/2017-3-30/4.png)

##mac安装
*****************
同上1,2步骤，但是双击安装包之后无法安装成功，这个需要权限，按住control键之后，再点击pkg文件，这个时候会弹出安装程序的界面，选择打开，就可以完成安装了。
![git install](/blog/img/2017-3-30/5.png)

**打开终端，生成SSH key**
```
cd ~/.ssh
ssh-keygen -t rsa -C jonezhang86@gmail.com
```
**copy SSh key**
```
vim id_rsa.pub
```
