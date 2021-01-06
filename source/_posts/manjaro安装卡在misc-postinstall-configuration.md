---
title: manjaro安装卡在misc postinstall configuration
date: 2021-01-06 21:44:45
tags:
- Linux
categories:
- 报错笔记
---
这个问题是因为安装完毕后执行了pacman-mirrors，有些国外的镜像源连接太慢太慢所以几乎要等几个小时。
<!--more-->

解决方法就是断网，过一会应该会好。

或者直接`ps -ef | grep pacman-mirrors`找到对应进程kill掉。

安装好重启后可以用`sudo pacman-mirrors -i -c China -m rank`命令重新排序下更新源
