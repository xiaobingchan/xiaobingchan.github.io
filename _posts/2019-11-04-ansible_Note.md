---
layout: post
title: ansible安装及使用
date: 2019-11-04
tags: 自动化工具
---


### 介绍

 　Kubernetes Dashboard 是一个管理Kubernetes集群的全功能Web界面，旨在以UI的方式完全替代命令行工具（kubectl 等）

### 安装环境
   Linux系统版本：CentOS Linux release 7.6.1810 (Core)
   Docker版本：19.03.4
 　Kubernetes 版本：v1.15.0

### 第一步，安装最新版Docker

```     
wget 

https://files.pythonhosted.org/packages/41/a8/41ac6efd240cde4d98068dd1b4a5172fea5dcee58d4f3496f75e40b

927c6/supervisor-4.0.4.tar.gz
tar -xzvf supervisor-4.0.4.tar.gz
cd supervisor-4.0.4
python3 setup.py build
python3 setup.py install
/data/ioszdhyw/soft/python3_7/bin/echo_supervisord_conf > /etc/supervisord.conf

cat >> /etc/supervisord.conf << EOF
[program:api]
directory = /data/ioszdhyw/soft/python3_7/bin
command = python3 /home/nfdw/tornado/aomm-jobservice/webSocker.py 1001
autostart = true
startsecs = 5
autorestart = true
startretries = 3
user = root
redirect_stderr = true
stdout_logfile_maxbytes = 20MB
stdout_logfile_backups = 10
stdout_logfile = /data/ioszdhyw/logs/sup_flume_stdout.log
stopasgroup=true
[program:api2]
directory = /data/ioszdhyw/soft/python3_7/bin
command = python3 /home/nfdw/tornado/aomm-jobservice/webSocker.py 1002
autostart = true
startsecs = 5
autorestart = true
startretries = 3
user = root
redirect_stderr = true
stdout_logfile_maxbytes = 20MB
stdout_logfile_backups = 10
stdout_logfile = /data/ioszdhyw/logs/sup_flume_stdout.log
stopasgroup=true
EOF

mkdir -p /data/ioszdhyw/logs/
firewall-cmd --zone=public --add-port=1001/tcp --permanent
firewall-cmd --zone=public --add-port=1002/tcp --permanent
firewall-cmd --reload
/data/ioszdhyw/soft/python3_7/bin/supervisord -c /etc/supervisord.conf
/data/ioszdhyw/soft/python3_7/bin/supervisorctl update
/data/ioszdhyw/soft/python3_7/bin/supervisorctl reload
/data/ioszdhyw/soft/python3_7/bin/supervisorctl restart all
```    
