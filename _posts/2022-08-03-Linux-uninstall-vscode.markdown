---
layout: post
title:  "Linux卸载VS Codee"
date:   2022-08-03 8:49:52 +0800
categories:  Linux
tags: vs code
---
查看该软件的安装位置：
```bash
$  whereis code
 ```
 
卸载vscode的方法有很多，选其中之一即可。具体如下：
 
(1) 通过 apt-get 方式安装的,删除时会提示确认：
```bash
$  sudo apt-get remove code           # 只是卸载，保留配置
#或
$  sudo apt-get --purge remove code   # 彻底清除，包括配置
#或
$  sudo apt-get purge  code           # 也是彻底清除
 ```
 
(2) 通过 dpkg 方式安装的,删除时将没有确认提示：
```bash
$  sudo dpkg -r code                  # 只是卸载，保留配置
#或
$  sudo dpkg --remove  code           # 只是卸载，保留配置
#或
$  sudo dpkg -r code                  # 彻底清除，包括配置
#或
$  sudo dpkg --purge  code            # 彻底清除，包括配置
```
