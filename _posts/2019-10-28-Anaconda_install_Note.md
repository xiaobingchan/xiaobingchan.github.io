---
layout: post
title: CentOS 7 搭建部署Python环境和包管理工具Anaconda 和 基本使用方法
date: 2019-10-23
tags: 管理工具
---


### 介绍

 　Anaconda是一个开源的包、环境管理器，可以用于在同一个机器上安装不同版本的软件包及其依赖，并能够在不同的环境之间切换，参考 [官网简介](https://www.anaconda.com/)

### 第1步：下载最新的Anaconda安装包
 　[Anaconda官网下载](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)
```     
$ wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-5.2.0-Linux-x86_64.sh
```    

### 第2步：安装anaconda

```  
$ bash Anaconda3-5.2.0-Linux-x86_64.sh
```  

### 第3步：查看anaconda版本和配置清华源
```  
$ conda -V
$ conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
```  

### 第4步：创建Python环境和安装pip包依赖
```  
$ conda create --name python27 python=2.7 # 创建python 3.5 的环境
$ conda env list                          # 列出当前anaconda环境
$ source activate python27                # 进入python35环境
(python35) $ conda install matplotlib                #  安装matplotlib包
(python35) $ conda list                              # 列出环境已安装的python包
(python35) $ source deactivate                       # 退出python35环境
$ conda remove --name python35 --all      # 删除python35环境
```  

 　Windows 创建环境命令：
```  
C:\Users\Administrator> activate python27  #  进入python27环境
C:\Users\Administrator> deactivate         #  退出python27环境
```  

### 常见报错 
 　PackagesNotFoundError: The following packages are not available from current channels，解决方法
```  
(python35) $  anaconda search -t conda uWSGI
(python35) $  anaconda show zegami/uwsgi
(python35) $  conda install --channel https://conda.anaconda.org/zegami uwsgi
``` 
 　