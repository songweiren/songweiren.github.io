---
layout: post
title:  "ubuntu16.04安装chrome谷歌浏览器"
date:   2018-01-21 22:49:52 +0800
categories: Linux
---
1、启动终端
```
sudo wget http://www.linuxidc.com/files/repo/google-chrome.list -P /etc/apt/sources.list.d/
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -
sudo apt-get update
sudo apt-get install google-chrome-stable
/usr/bin/google-chrome-stable
```
2、为chrom安装flash播放器
屏幕右上角，“系统设置”–>“软件和更新”–>“其它软件”
勾选“Canonical合作伙伴”
点击“关闭”
终端输入
```
sudo apt install adobe-flashplugin
```