---
layout: post
title: Linux搭建部署（升级）最新版Docker
date: 2019-10-23
tags: 容器
---


### 介绍

 　Docker应用功能介绍，参考 [官网简介](https://docs.docker.com/engine/docker-overview/)，本文的 Docker 部署流程主要参考了 Docker 官方安装教程，基于 Linux CentOS，[原文链接](https://docs.docker.com/install/linux/docker-ce/centos/)

> * 一般不建议直接使用 yum 安装 Docker ，因为 yum 安装的 Docker 一般版本偏低，无法识别新的 Dockerfile 语法，如 “as、from” 等等
> * 高版本的 Docker 往往对 K8S 有更好的性能表现

### 部署方法1：在线安装最新版

yum 安装指定版本 Docker

```     
$ yum install -y yum-utils device-mapper-persistent-data lvm2
$ yum-config-manager  --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
$ yum list docker-ce --showduplicates | sort -r
$ yum install docker-ce-18.06.3.ce   
```    

设置Docker开机启动，启动服务

```    
$ systemctl enable docker
$ systemctl start docker
```   

测试Docker是否运行正常

```    
$ docker pull hello-world
$ docker run hello-world
```


![](/images/posts/jekyll/image1.png)

so easy !
