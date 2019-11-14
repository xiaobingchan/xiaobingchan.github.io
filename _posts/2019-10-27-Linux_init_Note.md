---
layout: post
title: Linux CentOS系统安装初始化 使用优化技巧
date: 2019-10-27
tags: 初始化
---

### 介绍

 　Linux 系统安装后并不满足实用需要，需要进行初始化，即对服务器配置的进行基本调整和一些基础软件的安装

   0，网卡配置外网访问
```     
$  vi /etc/sysconfig/network-scripts/ifcfg-ens33
# 编辑ifcfg-ens33网卡配置文件，ONBOOT参数将“no”改为“yes”
$  service network restart
```  
   查看网卡是否正常获取ip地址：
```  
$  ip addr
```  
   测试网络是否能访问：
```  
[root@localhost ~]# ping www.baidu.com
PING www.baidu.com (182.61.200.7) 56(84) bytes of data.
64 bytes from 182.61.200.7 (182.61.200.7): icmp_seq=1 ttl=128 time=73.9 ms
64 bytes from 182.61.200.7 (182.61.200.7): icmp_seq=2 ttl=128 time=71.1 ms
64 bytes from 182.61.200.7 (182.61.200.7): icmp_seq=3 ttl=128 time=1217 ms
```  

 　1，关闭防火墙
```     
$ setenforce 0
$ vi /etc/sysconfig/selinux
# 编辑 /etc/sysconfig/selinux 将 “ SELINUX=enforcing ” 改成 “ SELINUX=disabled ” ，
注释掉 “ SELINUXTYPE=targeted ”
```  
 　2，添加22端口访问ssh
 　CentOS 7 添加 22 端口 允许访问
```     
$ firewall-cmd --zone=public --add-port=22/tcp --permanent    
$ firewall-cmd --reload
```    
 　CentOS 6 添加 22 端口 允许访问
```     
$  vi /etc/sysconfig/iptables
# 编辑 /etc/sysconfig/iptables 添加 “ -A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT ”
$  service iptables restart
```  

 　3，安装基础软件：
```    
$  yum install -y net-tools wget unzip git
``` 

 　4，配置阿里yum源和扩展源，cd /etc/yum.repos.d 目录备份原本的resp文件，根据centos版本下载对应的repo  `！！！千万不要搞错版本，后果自负！！！！`
```  
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
sed -i '/aliyuncs/d' /etc/yum.repos.d/CentOS-Base.repo
sed -i 's/$releasever/7/g' /etc/yum.repos.d/CentOS-Base.repo
yum clean all
yum makecach
``` 
 　安装拓展源
```  
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
sed -i '/aliyuncs/d' /etc/yum.repos.d/epel.repo
yum clean all
yum makecache
yum install epel-release
```  

 　5，ntp服务同步时间，校正系统时间　
``` 
[root@localhost ~]# yum install -y ntpdate
[root@localhost ~]# ntpdate us.pool.ntp.org
27 Oct 16:32:56 ntpdate[17007]: step time server 107.15.121.121 offset 7360.683491 sec
[root@localhost ~]# date
2019年 10月 27日 星期日 16:33:07 CST
``` 

**提高远程ssh服务器**

> 编辑 /etc/ssh/sshd_config 将 “GSSAPIAuthentication yes” 改为 “GSSAPIAuthentication no” ，加入 “UseDNS no”，重启服务 “systemctl restart sshd” （Centos6 重启服务 ：/etc/init.d/sshd restart ）

**开机禁用防火墙**

> CentOS7命令： 
`systemctl stop firewalld && systemctl disable firewalld`

> CentOS6命令： 
`/etc/init.d/iptables stop && chkconfig firewalld off`
