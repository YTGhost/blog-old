---
title: wepy在WebStorm配置代码高亮
date: 2021-01-19 19:30:01
tags:
- 微信小程序
- wepy
categories:
- 配置笔记
---
关于wepy在WebStorm代码高亮的一些配置
<!--more-->
配置环境：
- Window10
- WebStorm 2020.3.1

## 支持wpy
window下：`File` - `Settings` - `Editor` - `File Types`，进行添加

![](https://image.hihia.top/Screenshot/20210119193818.png)

## 高亮config和wxs标签
`Namespace`用`http://www.w3.org/1999/html`

![](https://image.hihia.top/Screenshot/20210119194725.png)
<img src="https://image.hihia.top/Screenshot/20210119194906.png" alt="" style="zoom: 67%;" />
<img src="https://image.hihia.top/Screenshot/20210119195000.png" alt="" style="zoom:67%;" />

## 安装插件
![](https://image.hihia.top/Screenshot/20210119210140.png)

<!-- ## 配置支持rpx
添加custom
![](https://image.hihia.top/Screenshot/20210119195140.png)
![](https://image.hihia.top/Screenshot/20210119195227.png)

Program: `sed`

Arguments（macOS）: `-i "" s/"\ rpx"/rpx/g $FilePath$`

Arguments（Windows）: `-i s/"\ rpx"/rpx/g $FilePath$`

Output paths to refresh: `$FilePath$`

> macOS有自带sed，Windows没有sed，可以在这下载[sed-windows](https://github.com/mbuilov/sed-windows)，如果有安装Git的话，可以直接用Git目录下的sed，一般在/Git/usr/bin目录下 -->