---
layout: post
title: CentOS 7 搭建部署Kubernetes Dashboard 
date: 2019-11-04
tags: 容器
---


### 介绍

 　Kubernetes Dashboard 是一个管理Kubernetes集群的全功能Web界面，旨在以UI的方式完全替代命令行工具（kubectl 等）

### 安装环境
   Linux系统版本：CentOS Linux release 7.6.1810 (Core)
   Docker版本：19.03.4
 　Kubernetes 版本：v1.15.0

### 第一步，安装最新版Docker

参考博客1：https://www.cnblogs.com/bluersw/p/11713468.html
参考博客2：https://www.jianshu.com/p/5ff6e26d1912

systemctl disable firewalld
systemctl stop firewalld

hostnamectl set-hostname master  

yum install -y ntp
ntpdate ntp1.aliyun.com

yum update

yum install wget

yum install -y yum-utils device-mapper-persistent-data lvm2

cd /etc/yum.repos.d/

yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

yum -y install docker-ce


cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
        http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF


yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

cd /etc/sysctl.d/

cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sysctl --system

systemctl start docker
systemctl enable docker

vi /etc/fstab
注释swap分区
# /dev/mapper/centos-swap swap                    swap    defaults        0 0

swapoff -a

systemctl enable kubelet


docker pull bluersw/kube-apiserver:v1.16.2 #替代docker pull k8s.gcr.io/kube-apiserver:v1.16.2
docker tag bluersw/kube-apiserver:v1.16.2 k8s.gcr.io/kube-apiserver:v1.16.2
docker pull bluersw/kube-controller-manager:v1.16.2 #替代docker pull k8s.gcr.io/kube-controller-manager:v1.16.2
docker tag bluersw/kube-controller-manager:v1.16.2 k8s.gcr.io/kube-controller-manager:v1.16.2
docker pull bluersw/kube-scheduler:v1.16.2 #替代docker pull k8s.gcr.io/kube-scheduler:v1.16.2
docker tag bluersw/kube-scheduler:v1.16.2 k8s.gcr.io/kube-scheduler:v1.16.2
docker pull bluersw/kube-proxy:v1.16.2 #替代docker pull k8s.gcr.io/kube-proxy:v1.16.2
docker tag bluersw/kube-proxy:v1.16.2 k8s.gcr.io/kube-proxy:v1.16.2
docker pull bluersw/pause:3.1 #替代docker pull k8s.gcr.io/pause:3.1
docker tag bluersw/pause:3.1 k8s.gcr.io/pause:3.1
docker pull bluersw/etcd:3.3.15-0 #替代docker pull k8s.gcr.io/etcd:3.3.15-0
docker tag bluersw/etcd:3.3.15-0 k8s.gcr.io/etcd:3.3.15-0
docker pull bluersw/coredns:1.6.2 #替代docker pull k8s.gcr.io/coredns:1.6.2
docker tag bluersw/coredns:1.6.2 k8s.gcr.io/coredns:1.6.2
docker pull bluersw/flannel:v0.11.0-amd64 #替代 docker pull quay.io/coreos/flannel:v0.11.0-amd64
docker tag bluersw/flannel:v0.11.0-amd64 quay.io/coreos/flannel:v0.11.0-amd64

kubeadm init  --kubernetes-version=v1.16.2 --apiserver-advertise-address=127.0.0.1 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.1.0.0/16

#把密钥配置加载到自己的环境变量里
export KUBECONFIG=/etc/kubernetes/admin.conf

#每次启动自动加载$HOME/.kube/config下的密钥配置文件（K8S自动行为）
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
kubectl get -A pods -o wide

wget https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml

docker pull mirrorgooglecontainers/kubernetes-dashboard-amd64:v1.10.1
docker tag mirrorgooglecontainers/kubernetes-dashboard-amd64:v1.10.1 k8s.gcr.io/kubernetes-dashboard-amd64:v1.10.1
docker rmi mirrorgooglecontainers/kubernetes-dashboard-amd64:v1.10.1

http://118.89.23.220:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/


[root@bogon ~]# kubectl proxy --address='0.0.0.0' --accept-hosts='^*$' --port=8001

```  
   访问 http://118.89.23.220:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
