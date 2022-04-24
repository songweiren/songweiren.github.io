---
layout: post
title:  "一键修改电源选项脚本"
date:   2022-04-24 18:49:52 +0800
categories: Windows  
tags: Windows
---
##### 一键修改Windows电源选项脚本
```powershell
@echo off
setlocal enabledelayedexpansion
:LOOP
ECHO Please select one of following option:
ECHO [1] Recovery Defult Value
ECHO [2] Never turn off monitor
SET /p ver=Choice:

IF %ver% equ 1 (
	GOTO DEFAULT
) ELSE IF %ver% equ 2 (
	GOTO NEVER
) ELSE (
	GOTO LOOP
)
:DEFAULT
ECHO monitor timeout 10 min
powercfg /X -monitor-timeout-ac 10
ECHO standby timeout 30 min
powercfg /X -standby-timeout-ac 30
GOTO END
:NEVER
echo never turn off monitor
powercfg /X -monitor-timeout-ac 0
echo never standby
powercfg /X -standby-timeout-ac 0
GOTO END
:END
PAUSE
:: powercfg /X commond
::  monitor-timeout-ac
::   monitor-timeout-dc
::   disk-timeout-ac
::   disk-timeout-dc
::   standby-timeout-ac
::   standby-timeout-dc
::   hibernate-timeout-ac
::   hibernate-timeout-dc
::
:: <VALUE>      指定新值(分钟)。
```