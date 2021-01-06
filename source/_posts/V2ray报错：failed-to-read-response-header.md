---
title: V2ray报错：failed to read response header
date: 2021-01-06 21:40:58
tags:
- Linux
categories:
- 报错笔记
---
查了查资料发现竟然是系统时间与本地时间不一致造成的，同步一下系统时间即可

```shell
# Manjaro上是这样
sudo ntpdate time.nist.gov
# debian和centos没有自带，需要先安装
apt-get install ntpdate
yum install ntpdate
```