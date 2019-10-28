---
layout: post
title: CentOS 7 部署 系统自带python2.7的pip依赖 、 Go环境、nodejs环境、 jdk环境
date: 2019-10-23
tags: 管理工具
---


### 介绍
   
 　pip(python2) 部署 [参考链接](https://www.cnblogs.com/ssyfj/p/9172584.html)
   
 　go 部署 [参考链接](https://www.cnblogs.com/nickchou/p/10934025.html)
   
 　jdk部署 [参考链接](https://blog.csdn.net/myhes/article/details/83276053)
   
 　nodejs 部署 [参考链接](https://www.runoob.com/nodejs/nodejs-install-setup.html)
   

###  1，给CentOS系统自带的python安装pip

 　[setuptools发布版本](https://pypi.org/project/setuptools/#history)

 　[pip发布版本](https://pypi.org/project/pip/#history)


```     
$ wget --no-check-certificat  https://pypi.python.org/packages/source/s/setuptools/setuptools-2.0.tar.gz
$ tar zxf setuptools-2.0.tar.gz
$ cd setuptools-2.0
$ python setup.py install
$ cd  ..
$ wget https://files.pythonhosted.org/packages/00/9e/4c83a0950d8bdec0b4ca72afd2f9cea92d08eb7c1a768363f2ea458d08b4/pip-19.2.3.tar.gz --no-check-certificate
$ tar -xzvf pip-19.2.3.tar.gz
$ cd pip-19.2.3
$ python setup.py install
$ python -m pip install --upgrade pip
```    

### 2，免编译搭建Go开发环境

 　下载最新版Go免编译包，[下载地址](https://golang.org/dl/)

```  
$ wget https://dl.google.com/go/go1.12.5.linux-amd64.tar.gz
$ tar -xzvf go1.12.5.linux-amd64.tar.gz -C /usr/local/

$ vi /etc/profile
export PATH=$PATH:/usr/local/nodejs/bin
$ source /etc/profile

```  

### 3，免编译搭建npm开发环境

 　下载最新版npm免编译包，[下载地址](https://nodejs.org/dist/)

```  
$ wget https://nodejs.org/dist/v13.0.1/node-v13.0.1-linux-x64.tar.gz
$ tar -xzvf node-v13.0.1-linux-x64.tar.gz -C /usr/local/
$ mv /usr/local/node-v13.0.1-linux-x64/ /usr/local/nodejs/

$ vi /etc/profile
export PATH=$PATH:/usr/local/nodejs/bin
$ source /etc/profile

$ /usr/local/nodejs/bin/node -v

```  

### 4，jdk部署

 　下载jdk，[下载地址](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)，选择jdk版本，点击 “download jdk ”，同意协议，下载 jdk-8u231-linux-x64.rpm

```  
$ rpm -ivh /data/soft/jdk-8u152-linux-x64.rpm

$ vi /etc/profile
export JAVA_HOME=/usr/java/jdk1.8.0_152
export CLASSPATH=%JAVA_HOME%/lib:%JAVA_HOME%/jre/lib
export PATH=$PATH:$JAVA_HOME/bin
$ source /etc/profile

$ java --version
``` 
