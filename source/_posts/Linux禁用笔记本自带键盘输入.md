---
title: Linux禁用笔记本自带键盘输入
date: 2021-01-06 21:49:01
tags:
- Linux
categories:
- 配置笔记
---
当我们想外接键盘并把键盘放到Linux系统笔记本上来使用时，该怎么禁用键盘输入呢？
<!--more-->
``` shell
# 这里我们需要安装xinput，这里作为示例使用pacman来安装
sudo pacman -Sy xinput
# 搜索出来的安装列表中可以选择extra/xorg-xinput安装

# 如何禁用：
# 列出设备列表
xinput list
# 找到AT Translated Set 2 keyboard对应的id，设置值为0即可禁用
# 禁用
xinput set-prop <id> "Device Enabled" 0
# 重新启用
xinput set-prop <id> "Device Enabled" 1
```