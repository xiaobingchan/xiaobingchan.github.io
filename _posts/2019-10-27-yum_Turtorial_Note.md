---
layout: post
title: yum 使用教程
date: 2019-10-27
tags: 部署
---

### 介绍

 　众所周知，Redhat和Fedora的软件安装命令是rpm。需要手动寻找安装该软件所需要的一系列依赖关系，yum的诞生很好解决了以上的问题，下面有几个实用的yum小技巧和大家分享。
   
 　0，搭建阿里 yum 源和 扩展源
 
```  
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
sed -i '/aliyuncs/d' /etc/yum.repos.d/CentOS-Base.repo
sed -i 's/$releasever/7/g' /etc/yum.repos.d/CentOS-Base.repo
yum clean all
yum makecach
``` 
 　
 　安装扩展源
 
```  
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
sed -i '/aliyuncs/d' /etc/yum.repos.d/epel.repo
yum clean all
yum makecache
yum install epel-release
```  

 　1，yum 报错 “This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.”

```     
$  [root@bogon ~]# yum install wget
   Loaded plugins: product-id, search-disabled-repos, subscription-manager
   This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
```  

 　解决方法：
```    
$ vi /etc/yum/pluginconf.d/subscription-manager.conf
# 将 “enabled=1” 改为 “enabled=0”
```    

 　2， yum 报错 There are no enabled repos. Run yum repolist all to see the repos you have. You can enable repos with yum-config-manager --enable <repo>
 
 　解决方法：下载阿里云对应的 [repo文件](https://mirrors.aliyun.com/repo/Centos-7.repo/) ，并上传至 /etc/yum.repos.d/ 目录 命名为 CentOS-Base.repo

```    
$ sed -i '/aliyuncs/d' /etc/yum.repos.d/CentOS-Base.repo
$ sed -i 's/$releasever/7/g' /etc/yum.repos.d/CentOS-Base.repo
$ yum clean all
$ yum makecache
```    

 　3， yum 报错 “ GPG key retrieval failed: ”
 
 　解决方法，yum指令后附带 “--nogpgcheck” ：
```    
$ yum install -y 包名 --nogpgcheck
``` 
   
   或是 CentOS-Base.repo 文件的  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release 改为真实可用的路径，或是设置 “gpgcheck=0”


 　4，搭建本地的dvd iso 镜像源
```   
$ mount -o  loop /data/soft/rhel-server-7.2-x86_64-dvd.iso /mnt

#  编辑 /etc/yum.repos.d/Server.repo 文件加入
[Server]
name=MyRPM
baseurl=file:///mnt
enabled=1
gpgcheck=0

```   

 　5，构造本地rpm包组成的diy yum源
```   
$ yum install createrepo            # 安装 yum 源制作工具
$ createrepo /home/cepuser/yumrepo  # 目录 /home/cepuser/yumrepo 放置需要依赖的 rpm 包

#  编辑 /etc/yum.repos.d/local.repo 文件加入
[local]
name=local
baseurl=file:///home/cepuser/yumrepo
gpgcheck=1
enabled=1
EOF

$ yum clean all
$ yum makecache
```   
