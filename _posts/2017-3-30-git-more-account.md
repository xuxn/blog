---
categories: learn
layout: post-detail
title:  "git 多账号配置"
date:   2017-01-19 17:41:04 +0800
---

# git 多账号配置

### **1. 创建github对应的sshkey**

```
ssh-keygen -t rsa -C email
```

### **2. 创建github对应的sshkey，命名为id_rsa_oschina，密码 123456** 

![git config](/blog/img/2017-3-30/6.png)

### **3. 访问的.ssh目录, 打开id_rsa_oschina.pub,copy SSh key 到github**

![git config](/blog/img/2017-3-30/8.png)

![git config](/blog/img/2017-3-30/7.png)

### **4. 配置config文件**

![git config](/blog/img/2017-3-30/9.png)