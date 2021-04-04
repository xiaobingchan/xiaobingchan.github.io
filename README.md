## Quick Start

主题地址：https://themes.gohugo.io/

视频上传：https://streamja.com/
图片上传：https://imgchr.com/

公众号模板：https://md.qikqiak.com/
公众号排版：https://www.mdnice.com/

## Quick Start

The simplest way is to start with the example site coming with this theme, then you can play around and add your own stuff.

```
$ wget https://github.com/gohugoio/hugo/releases/download/v0.82.0/hugo_0.82.0_Linux-64bit.tar.gz
$ tar -xzvf hugo_0.82.0_Linux-64bit.tar.gz 
$ mv hugo /usr/bin/
$ hugo version

$ mkdir test
$ cd test
$ mkdir themes
$ cd themes
$ git clone https://github.com/xiaobingchan/xiaobingchan.github.io
$ cd xiaobingchan.github.io
$ systemctl stop firewalld
$ nohup hugo server --baseURL=ec2-18-216-109-197.us-east-2.compute.amazonaws.com --bind=0.0.0.0 --port=80 &
```
