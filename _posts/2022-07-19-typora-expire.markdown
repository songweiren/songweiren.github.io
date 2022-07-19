---
layout: post
title:  "一键修改电源选项脚本"
date:   2022-07-19 18:49:52 +0800
categories: Windows  
tags: Windows
---
##### typora过期打开脚本

typora过期打开脚本
```bat
@echo off
for /f "delims= " %%d in ('echo %date%') do (set "now=%%d")
date 2020/1/1
echo ChangeDateTime:2020/1/1
start C:\Progra~1\Typora\Typora.exe
::start "" "C:\Program Files\Typora\Typora.exe"
echo Start Typora...
choice /t 1 /d y /n >nul
date %now%
echo restore time
exit
```
