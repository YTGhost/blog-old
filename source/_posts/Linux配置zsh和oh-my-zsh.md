---
title: Linux配置zsh和oh-my-zsh
date: 2021-01-06 22:04:34
tags:
- Linux
categories:
- 配置笔记
---
这里的安装以Centos7为例
<!--more-->
## 安装zsh

`yum -y install zsh`

查看是否安装成功：

<img src="https://image.hihia.top/Screenshot/image-20201115144159162.png" alt="" style="zoom: 50%;" />

切换shell为zsh：

`chsh -s /bin/zsh`

<img src="https://image.hihia.top/Screenshot/image-20201115144305088.png" alt="" style="zoom:50%;" />

之后需要重启服务器：

`sudo reboot`

重启后查看是否切换成功：

`echo $SHELL`

<img src="https://image.hihia.top/Screenshot/image-20201115144516176.png" alt="" style="zoom:50%;" />

## 安装oh-my-zsh

先确保已经安装过git：

`sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

![](https://image.hihia.top/Screenshot/image-20201115145004202.png)

这个时候oh-my-zsh就安装成功啦

## 插件安装

没有安装插件的oh-my-zsh就好比是没有安装拓展的vscode一样，这里我会推荐几个常用的插件。

### zsh-autosuggestions

插件github地址：https://github.com/zsh-users/zsh-autosuggestions

安装命令：`git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`

在`~/.zshrc`中配置：

<img src="https://image.hihia.top/Screenshot/image-20201115145813429.png" alt="" style="zoom:50%;" />

保存后`source ~/.zshrc `即可

### Zsh-syntax-highlighting

这个是用来语法高亮的

插件github地址：https://github.com/zsh-users/zsh-syntax-highlighting

安装命令：`git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`

在`~/.zshrc`中配置：

<img src="https://image.hihia.top/Screenshot/image-20201115150533626.png" alt="" style="zoom:50%;" />

保存后`source ~/.zshrc `即可

**更多插件持续更新中...**