---
layout: post
title:  "bat文件语法检查脚本"
date:   2024-09-10 20:49:52 +0800
categories: bat  
tags: bat
---

检查bat文件中是否有语法错误
``` bash
@echo off
setlocal enabledelayedexpansion
set "DIRECTORY=\path\"
 
for /r "%DIRECTORY%" %%f in (*.bat) do (
    echo Checking syntax of %%~nxf
    cmd /c ""%%~f" >nul 2>nul" && (
        echo %%~nxf syntax is correct.
    ) || (
        echo Error: %%~nxf has syntax errors.
        cmd /c ""%%~f"">nul 2>syntax_errors.txt"
        type syntax_errors.txt
        del syntax_errors.txt
    )
)
 
endlocal
```
