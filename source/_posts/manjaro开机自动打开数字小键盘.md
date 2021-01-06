---
title: manjaro开机自动打开数字小键盘
date: 2021-01-06 21:54:07
tags:
- Linux
categories:
- 配置笔记
---
manjaro怎么开机的时候就启用数字小键盘呢？
```shell
sudo vim /etc/sddm.conf
# 找到[General]下的
Numlock=none
# 改成
Numlock=on
# 保存后重启即可
```

