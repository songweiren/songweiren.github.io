---
layout: post
title:  "bat文件注意事项"
date:   2024-09-11 08:49:52 +0800
categories: bat  
tags: bat
---
bat文件注意事项：
- set value中等号前后不能有空格
``` bash
:INFO
echo ******************Setup for Project******************
echo [1] Clone config files:
echo Run CMD: conan config install ssh://%USERNAME%@eps-sw-gerrit-ssh.volvocars.net:29418/conan/cfg --type git
echo [2] Install dependency and Generte project script:
echo Run CMD: conan install .. --update --build=missing
echo [3] Build Project and Generte C Codes:
echo Run CMD: conan build ..
echo [INFO]Current Path: %cd% 
set /p sel=Please input number:
```
- if else 后括号
``` bash
if %sel% == 1 (
REM echo enter %sel%
goto CLONE_CONFIG
) else if %sel% == 2 (
REM echo enter %sel%
goto CONAN_INSTALL
) else if %sel% == 3 (
REM echo enter %sel%
goto CONAN_BUILD
) else (
echo [ERROR]Not Supported!
goto INFO
)
```
- 判断路径存在
``` bash
set "build_folder=%cd%\build"
if not exist %build_folder%\%NUL% (
  mkdir %build_folder%
) else (
echo "exist."
)
```
- 变量延迟
``` bash
setlocal enabledelayedexpansion 
``` 
- pending
- pending
- pending
- 
