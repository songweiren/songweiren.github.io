---
layout: post
title:  "Linux卸载多余内核"
date:   2022-09-14 8:49:52 +0800
categories:  Linux
tags: kenel
---
##### Debian和Ubuntu中删除多余的内核
查看当前的内核
```bash
uname -sr
```
查看已安装的内核
```bash
dpkg -l | grep linux-image | awk '{print$2}'
```
删除多余的内核
```bash
apt remove --purge linux-image-4.4.0-21-generic
```
重新检查

```bash
dpkg -l | grep linux-image | awk '{print$2}'
```
更新引导系统
```bash
update-grub
reboot
```

##### 卸载软件相关命令
先运行sudo apt-get remove mysql-\* 卸载mysql相关的所有应用，最后用斜杠加星号匹配是因为在zsh下
卸载的时候提示有一些lib依赖没有被使用，所以运行sudo apt autoremove清除无用的依赖
运行dpkg --list|grep mysql，查看mysql有哪些安装还没有完全卸载
然后运行sudo dpkg --purge加上面列出来的一个个软件包名，一个个的卸载
